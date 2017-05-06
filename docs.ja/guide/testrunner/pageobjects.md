name: pageobjects
category: testrunner
tags: guide
index: 6
title: WebdriverIO - Page Object Pattern
---

ページ オブジェクト パターン
===================

WebdriverIO の新しいバージョン (v4) は、ページ オブジェクト パターンのサポートを念頭に設計されています。「第一級オブジェクトとしてのエレメント」の原則を導入することにより、このパターンを使用して大規模なテスト スイートを構築することが可能になりました。ページ オブジェクトの作成に必要な追加パッケージはありません。`Object.create` は、必要なすべての必要な機能を提供することが判明しました。

- ページ オブジェクト間の継承
- エレメントの遅延ローディング
- メソッドとアクションのカプセル化

ページオブジェクトの目的は、実際のテストからページ情報を抽象化することです。理想的には、特定のページに固有のすべてのセレクタまたは特定の命令をページ オブジェクトに格納して、ページを完全に再設計した後もテストを実行できるようにする必要があります。

まずページと呼ばれるメイン ページ オブジェクトが必要です。
これは、すべてのページ オブジェクトが継承する、共通のセレクタやメソッドを含みます。
次のように、すべての子ページオブジェクトとは別に、プロトタイプ モデルを使用して`Page`を作成します。

```js
function Page () {
    this.title = 'My Page';
}

Page.prototype.open = function (path) {
    browser.url('/' + path)
}

module.exports = new Page()
```

もしくは、ES6 のクラスを使います。

```js
"use strict";

class Page {

    constructor() {
        this.title = 'My Page';
    }

    open(path) {
        browser.url('/' + path);
    }

}
module.exports = new Page();
```

私たちは常にページオブジェクトのインスタンスをエクスポートし、テストでは決してそのインスタンスを作成しません。
エンド・ツー・エンドのテストを書いているので、各httpリクエストがステートレスな構造であるのと同じように、ページは常にステートレスな構造であるとみなされます。
もちろん、ブラウザはセッション情報を運ぶことができ、したがって異なるセッションに基づいて異なるページを表示することができますが、これはページオブジェクト内に反映されるべきではありません。
これらの状態の変化は、実際のテストから浮かび上がるはずです。

では最初のページをテストしましょう。
デモをするため、我々はサンプルとして[Elemental Selenium](http://elementalselenium.com/) の [The Internet](http://the-internet.herokuapp.com)ウェブサイトを使用します。
[ログインページ](http://the-internet.herokuapp.com/login)用のページ オブジェクトの例を構築しましょう。
First step is to write all important selectors that are required in our `login.page` object as getter functions.
最初のステップでは、 `login.page` オブジェクトに必要なすべての重要なセレクタを `getter` 関数として記述します。
上で述べたように、`Object.create` メソッドを使用して、メインページのプロトタイプを継承しています。

```js
// login.page.js
var Page = require('./page')

var LoginPage = Object.create(Page, {
    /**
     * エレメントの定義
     */
    username: { get: function () { return browser.element('#username'); } },
    password: { get: function () { return browser.element('#password'); } },
    form:     { get: function () { return browser.element('#login'); } },
    flash:    { get: function () { return browser.element('#flash'); } },

    /**
     * メソッドの定義またはオーバーライド
     */
    open: { value: function() {
        Page.open.call(this, 'login');
    } },

    submit: { value: function() {
        this.form.submitForm();
    } }
});

module.exports = LoginPage;
```

ES6 のクラスを使うとこうなります。

```js
// login.page.js
"use strict";

var Page = require('./page')

class LoginPage extends Page {

    get username()  { return browser.element('#username'); }
    get password()  { return browser.element('#password'); }
    get form()      { return browser.element('#login'); }
    get flash()     { return browser.element('#flash'); }

    open() {
        super.open('login');
    }

    submit() {
        this.form.submitForm();
    }

}
module.exports = new LoginPage();
```

getter 関数でセレクターを定義するのは少し冗長に見えるかもしれませんが、本当に便利です。これらの関数は、実際にプロパティにアクセスしたときに評価され、オブジェクトを生成するときには評価されません。それを使って、アクションが実行される前に、常にエレメントをリクエストします。

WebdriverIO は、コマンドの最終結果を内部的に記憶します。element コマンドにアクションコマンドをチェーンすると、前のコマンドのエレメントが検索され、その結果を使用してアクションが実行されます。これでセレクタ (最初のパラメータ) の指定をせずにすみ、コマンドは次のように簡単になります。


```js
LoginPage.username.setValue('Max Mustermann');
```

これは、基本的に次と同じことです。

```js
var elem = browser.element('#username');
elem.setValue('Max Mustermann');
```

または

```js
browser.element('#username').setValue('Max Mustermann');
```

または

```js
browser.setValue('#username', 'Max Mustermann');
```

ページに必要な要素とメソッドをすべて定義したら、テストの作成を開始できます。ページオブジェクトを使用するために必要なのは、それを require することだけです。`Object.create` メソッドは、そのページのインスタンスを返してすぐに使用できるようにします。アサーションフレームワークを追加することで、テストをさらに表現力豊かにすることができます。


```js
// login.spec.js
var expect = require('chai').expect;
var LoginPage = require('../pageobjects/login.page');

describe('login form', function () {
    it('should deny access with wrong creds', function () {
        LoginPage.open();
        LoginPage.username.setValue('foo');
        LoginPage.password.setValue('bar');
        LoginPage.submit();

        expect(LoginPage.flash.getText()).to.contain('Your username is invalid!');
    });

    it('should allow access with correct creds', function () {
        LoginPage.open();
        LoginPage.username.setValue('tomsmith');
        LoginPage.password.setValue('SuperSecretPassword!');
        LoginPage.submit();

        expect(LoginPage.flash.getText()).to.contain('You logged into a secure area!');
    });
});
```

構造的な側面からは、スペックファイルとページオブジェクトを分離し、それらを異なるディレクトリに置くのが良いでしょう。さらに、各ページ オブジェクトの最後を`.page.js` とします。このようにして、`var LoginPage = require('../pageobjects/form.page');` と記述するとページ オブジェクトを require していることが良くわかります。
これは、WebdriverIO でページ オブジェクトを書く基本的な原則です。より複雑なページ オブジェクト構造を構築することもできます。
たとえば、モーダル用のページオブジェクトや、巨大なページオブジェクトをメインのページ オブジェクトからセクションのオブジェクトに分割することなどがあります。
このパターンは実際のテストからページ情報をカプセル化する機会を多く与えます。こうして、プロジェクトとテストの数が増えたときにテストスイートを構造化して明確に保つことが重要です。

GitHubの [example フォルダ](https://github.com/webdriverio/webdriverio/tree/master/examples/pageobject)には、このサンプルといくつかのページ オブジェクトのサンプルがあります。
