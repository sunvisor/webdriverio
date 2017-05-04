name: frameworks
category: testrunner
tags: guide
index: 2
title: WebdriverIO - Test Runner Frameworks
---

Frameworks
==========

wdioランナーは現在、[Mocha](http://mochajs.org/)、[Jasmine](http://jasmine.github.io/) (v2.0) 、[Cucumber](https://cucumber.io/)  をサポートしています。 WebdriverIOに各フレームワークを統合するには、NPM上のアダプター・パッケージをダウンロードしてインストールする必要があります。これらのパッケージは、WebdriverIO がインストールされているのと同じ場所にインストールする必要があります。WebdriverIO をグローバルにインストールした場合は、アダプタパッケージもグローバルにインストールされていることを確認してください。

スペックファイルまたはステップ定義内で、グローバル変数 `browser` を使用して webdriver インスタンスにアクセスできます。Selenium セッションを開始または終了する必要はありません。これは wdio テストランナーによって処理されます。

## Mocha を使う

まず、NPM からアダプター パッケージをインストールする必要があります。

```sh
npm install wdio-mocha-framework --save-dev
```

Mocha を使いたい場合は、さらに表現が豊かなテストを行うためのアサーションライブラリ（例えば[Chai](http://chaijs.com)）をインストールする必要があります。 設定ファイルの before フックでそのライブラリを初期化します。

```js
before: function() {
    var chai = require('chai');
    global.expect = chai.expect;
    chai.Should();
}
```

設定できたら、次のような美しいアサーションを書くことができます。

```js
describe('my awesome website', function() {
    it('should do some chai assertions', function() {
        browser.url('http://webdriver.io');
        browser.getTitle().should.be.equal('WebdriverIO - Selenium 2.0 javascript bindings for nodejs');
    });
});
```

WebdriverIOは、Mocha の `BDD` （デフォルト）、`TDD`、および `QUnit` [インターフェイス](https://mochajs.org/#interfaces)をサポートしています。スペックをTDD言語で書きたければ、`mochaOpts` 設定の ui プロパティを`tdd` に設定してください。テストファイルは以下のように書きます。

```js
suite('my awesome website', function() {
    test('should do some chai assertions', function() {
        browser.url('http://webdriver.io');
        browser.getTitle().should.be.equal('WebdriverIO - Selenium 2.0 javascript bindings for nodejs');
    });
});
```

特定のMocha の設定を定義する場合は、設定ファイルに `mochaOpts` を追加して設定できます。すべてのオプションの一覧は、 [プロジェクトのWebサイト](http://mochajs.org/)にあります。

すべてのコマンドが同期して実行されているため、Mocha で非同期モードを有効にする必要はありません。 そのため、 `done` コールバックは使用できません。


```js
it('should test something', function () {
    done(); // throws "done is not a function"
})
```

非同期に何かを実行する場合は、 [`call`](/api/utility/call.html) コマンドまたは[カスタム コマンド](/guide/usage/customcommands.html)を使用できます。

## Jasmine を使う

まず、NPMからアダプター・パッケージをインストールする必要があります。

```sh
npm install wdio-jasmine-framework --save-dev
```

Jasmine はすでに WebdriverIO で使用できるアサーションメソッドを提供しています。したがって、別のものを追加する必要はありません。

### アサーションのインターセプト

Jasmineフレームワークでは、リザルトに応じてアプリケーションまたはWebサイトの状態を記録するために、各アサーションをインターセプトできます。 例えば、アサーションが失敗するたびにスクリーンショットを撮るのはとても便利です。`expectationResultHandler` には、実行する関数を取る `expectationResultHandler` というプロパティを追加できます。 関数のパラメータは、アサーションの結果に関する情報を提供します。 次の例は、アサーションが失敗した場合のスクリーンショットの取得方法を示しています。

```js
jasmineNodeOpts: {
    defaultTimeoutInterval: 10000,
    expectationResultHandler: function(passed, assertion) {
        /**
         * only take screenshot if assertion failed
         */
        if(passed) {
            return;
        }

        browser.saveScreenshot('assertionError_' + assertion.error.message + '.png');
    }
},
```

テストの実行を非同期にすることはできません。 コマンドの実行に時間がかかり、Webサイトの状態が変更される可能性があります。通常、2つのコマンドの後にスクリーンショットが撮られていますが、これはエラーに関する貴重な情報を提供します。

## Cucumber を使う

First you need to install the adapter package from NPM:

```sh
npm install wdio-cucumber-framework --save-dev
```

If you want to use Cucumber set the `framework` property to cucumber, either by adding `framework: 'cucumber'` to the [config file](/guide/testrunner/configurationfile.html) or by adding `-f cucumber` to the command line.

Options for Cucumber can be given in the config file with cucumberOpts. Check out the whole list of options [here](https://github.com/webdriverio/wdio-cucumber-framework#cucumberopts-options).

To get up and running quickly with Cucumber have a look on our [cucumber-boilerplate](https://github.com/webdriverio/cucumber-boilerplate) project that comes with all step definition you will probably need and allows you to start writing feature files right away.
