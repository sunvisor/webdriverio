layout: guide
name: guide
title: WebdriverIO - Developer Guide
---

# 開発者ガイド

WebdriverIO のドキュメントへようこそ。素早くスタートを切る手助けをしましょう。 問題に遭遇した場合は、[Gitter Channel](https://gitter.im/webdriverio/webdriverio) でヘルプや回答を見つけることができます。あるいは [Twitter](https://twitter.com/webdriverio) で見つけて下さい。また、このチュートリアルの後にサーバーの起動やテストの実行で問題が発生する場合は、プロジェクトディレクトリにサーバーとgeckodriverがあることを確認してください。そうでない場合は、以下の手順2および3で再ダウンロードしてください。

以下は、WebdriverIO スクリプトを起動して実行するための導入ガイドです。

## 最初のステップ

[Node.js](http://nodejs.org/) とJavaがすでにインストールされているとしましょう。まず、ブラウザ内で selenium コマンドを実行する selenium サーバを起動する必要があります。これを行うには、まずサンプルフォルダを作成します。

**1. シンプルなテストフォルダーを作成する**

```sh
$ mkdir webdriverio-test && cd webdriverio-test
```

*このフォルダにいる状態で*

次に、[selenium standalone server](http://docs.seleniumhq.org/download/) 
の最新バージョンをダウロードしましょう。

**2. selenium の最新バージョンをダウンロードする**

```sh
$ curl -O http://selenium-release.storage.googleapis.com/3.0/selenium-server-standalone-3.0.1.jar
```

**3. 環境にあった geckdriver の最新版をダウンロードし、プロジェクト ディレクトリに解凍する**

Linux 64 bit

```sh
$ curl -L https://github.com/mozilla/geckodriver/releases/download/v0.11.1/geckodriver-v0.11.1-linux64.tar.gz | tar xz
```

OSX

```sh
$ curl -L https://github.com/mozilla/geckodriver/releases/download/v0.11.1/geckodriver-v0.11.1-macos.tar.gz | tar xz
```

注: 他の geckodriver のリリースは、[ここ](https://github.com/mozilla/geckodriver/releases)から入手できます。

Start the server by executing the following:
次のコマンドを次項してコマンドを実行してサーバーを起動します。

**4. selenium standalone server を起動する**

```sh
$ java -jar -Dwebdriver.gecko.driver=./geckodriver selenium-server-standalone-3.0.1.jar
```

このコマンドは webdriver パス変数を設定して、Selenium がプロジェクトディレクトリに追加した geckdriver バイナリを使用し、Seleniumスタンドアロンサーバを起動します。

これをバックグラウンドで実行し、新しいターミナルウィンドウを開きます。次のステップでは、WebdriverIOをNPM経由でダウンロードします。

**5. WebdriverIO をダウンロードする**

```sh
$ npm install webdriverio
```

**6.次の内容のテストファイル（test.js）を作成します。**

```js
var webdriverio = require('webdriverio');
var options = {
    desiredCapabilities: {
        browserName: 'firefox'
    }
};

webdriverio
    .remote(options)
    .init()
    .url('http://www.google.com')
    .getTitle().then(function(title) {
        console.log('Title was: ' + title);
    })
    .end();
```

**7. テストファイルを実行する**
```sh
$ node test.js
```

次のように出力されます。

```sh
Title was: Google
```

おめでとう！WebdriverIO で最初の自動化スクリプトを実行しました。ノッチを上げて、実際のテストを作成しましょう。

## 大事なことをしましょう

（まだ実行していない場合は、プロジェクトのルートディレクトリに戻ります）

ここまではウォームアップでした。さらにテストランナーと一緒に WebdriverIO を実行しましょう。プロジェクトにおいて WebdriverIO で統合テストをする場合は、テストランナーを使用することをお勧めします。テストランナーには人生を楽にする多くの便利な機能があるからです。 最初のステップは、設定ファイルを作成することです。 これを行うには、設定ユーティリティを実行するだけです。

```sh
$ ./node_modules/.bin/wdio config
```

質問インターフェイスがポップアップします。 設定を簡単かつ迅速に作成することができます。何と答えていいか分からない時は、次のように答えましょう。

__Q: Where do you want to execute your tests?__<br>
A: _On my local machine_<br>
<br>
__Q: Which framework do you want to use?__<br>
A: _mocha_<br>
<br>
__Q: Shall I install the framework adapter for you?__<br>
A: _Yes_ (just press enter)<br>
<br>
__Q: Where are your test specs located?__<br>
A: _./test/specs/**/*.js_ (just press enter)<br>
<br>
__Q: Which reporter do you want to use?__<br>
A: _dot_ (just press space and enter)<br>
<br>
__Q: Shall I install the reporter library for you?__<br>
A: _Yes_ (just press enter)<br>
<br>
__Q: Do you want to add a service to your test setup?__<br>
A: none (just press enter, let's skip this for simplicity)<br>
<br>
__Q: Level of logging verbosity:__<br>
A: _silent_ (just press enter)<br>
<br>
__Q: In which directory should screenshots gets saved if a command fails?__<br>
A: _./errorShots/_ (just press enter)<br>
<br>
__Q: What is the base url?__<br>
A: _http://localhost_ (just press enter)<br>

それでおしまいです！ コンフィギュレータは必要なパッケージをすべてインストールし、wdio.conf.js という名前の設定ファイルを作成します。次のステップは、最初のスペックファイル（テストファイル）を作成することです。そのために、次のようなテストフォルダを作成します。

```sh
$ mkdir -p ./test/specs
```

次に、その新しいフォルダに単純なspecファイルを作成しましょう。

```js
var assert = require('assert');

describe('webdriver.io page', function() {
    it('should have the right title - the fancy generator way', function () {
        browser.url('http://webdriver.io');
        var title = browser.getTitle();
        assert.equal(title, 'WebdriverIO - Selenium 2.0 javascript bindings for nodejs');
    });
});
```
最後のステップは、テストランナーを実行することです。 次のように実行します。

```sh
$ ./node_modules/.bin/wdio wdio.conf.js
```

バンザイ！テストは合格し、WebdriverIO で統合テストを作成することができます。 興味があれば、より詳しいビデオオンボードのチュートリアル [learn.webdriver.io](http://learn.webdriver.io/) という独自のコースをチェックしてみてください。また、私たちのコミュニティには、たくさんの[ボイラープレート プロジェクト](/guide/getstarted/boilerplate.html)が集まっています。
