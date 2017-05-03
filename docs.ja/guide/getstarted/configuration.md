name: configuration
category: getstarted
tags: guide
index: 1
title: WebdriverIO - Configuration
---

# コンフィグレーション

WebdriverIO インスタンスを生成する場合は、適切な機能と設定を設定するためにいくつかのオプションを定義する必要があります。 `remote` メソッドを呼び出すときに次のようにします。

```js
var webdriverio = require('webdriverio');
var client = webdriverio.remote(options);
```

options オブジェクトを渡して Webdriver インスタンスを定義する必要があります。これは、WebdriverIO をスタンドアロンパッケージとして実行する場合にのみ必要であることに注意してください。 wdio テストランナーを使用している場合、これらのオプションは `wdio.conf.js` 設定ファイルに置きます。次のオプションが定義できます。

### desiredCapabilities

that helps you to create this object by clicking together your desired capabilities.

Selenium セッションで実行したい機能を定義します。使える機能のリストについては、[Selenium のマニュアル](https://github.com/SeleniumHQ/selenium/wiki/DesiredCapabilities)を参照してください。また、目的の機能を一緒にクリックしてこのオブジェクトを作成するのに役立つSauce Labsの[自動テストコンフィグレータ](https://wiki.saucelabs.com/display/DOCS/Platform+Configurator#/)も便利です。

さらなるサービス固有のオプションについては、 [クラウドサービスのドキュメント](/guide/usage/cloudservices.html)を参照してください。

タイプ: `Object`<br>
デフォルト: `{ browserName: 'firefox' }`<br>

**例:**

```js
browserName: 'chrome',    // options: `firefox`, `chrome`, `opera`, `safari`
version: '27.0',          // browser version
platform: 'XP',           // OS platform
tags: ['tag1','tag2'],    // specify some tags (e.g. if you use Sauce Labs)
name: 'my test'           // set name for test (e.g. if you use Sauce Labs)
pageLoadStrategy: 'eager' // strategy for page load
```

**詳細:**

`pageLoadStrategy` はSelenium [2.46.0](https://github.com/SeleniumHQ/selenium/blob/master/java/CHANGELOG#L494) に実装されており、Firefox上でのみ動作するようです。有効な値は次のとおりです。

`normal` - document.readyStateが 'complete' になるのを待ちます。デフォルトではこの値が使用されます。<br>
`eager` - 'complete' を待つのではなく、`document.readyState` が 'interactive' のときに待機を中止します。<br>
`none` - ページが読み込まれるのを待つことなく、すぐに待機を中止します。

### logLevel

ロギングレベル。

__verbose:__ 全てを記録します<br>
__silent:__ なにも記録しません<br>
__command:__ Selenium サーバへの URL を記録します (例: `[15:28:00]  COMMAND     GET      "/wd/hub/session/dddef9eb-82a9-4f6c-ab5e-e5934aecc32a/title"`)<br>
__data:__ リクエストのペイロードを記録します (例: `[15:28:00]  DATA                {}`)<br>
__result:__ Selenium サーバーからのリザルトを記録します (例: `[15:28:00]  RESULT              "Google"`)

タイプ: `String`<br>
デフォルト: *silent*<br>
オプション: *verbose* | *silent* | *command* | *data* | *result*

### logOutput

WebdriverIO のログをファイルにパイプします。ディレクトリを定義すると、WebdriverIO がログファイルのファイル名を生成しますし、書き込み可能なストリームを渡すと、すべてがリダイレクトされます（最後のものは wdio ランナーでは動作しません）。

タイプ: `String|writeable stream`<br>
デフォルト: *null*

### protocol

Selenium スタンドアロンサーバ（またはドライバ）と通信するときに使用するプロトコル。

タイプ: `String`<br>
デフォルト: *http*

### host

WebDriver サーバーのホスト。

タイプ: `String`<br>
デフォルト: *127.0.0.1*

### port

WebDriver サーバーのポート番号。

タイプ: `Number`<br>
デフォルト: *4444*

### path

WebDriver サーバーへのパス。

タイプ: `String`<br>
デフォルト: */wd/hub*

### baseUrl

baseUrl を設定すると、`url` コマンドの呼び出しを短縮できます。`url` パラメータが `/` で始まる場合は、ベースURLの先頭に追加されます。

タイプ: `String`<br>
デフォルト: *null*

### connectionRetryTimeout
Selenium サーバーへのリクエストのタイムアウト。

タイプ: `Number`<br>
デフォルト: *90000*

### connectionRetryCount
Selenium サーバーへのリクエストをリトライする数。

タイプ: `Number`<br>
デフォルト: *3*

### coloredLogs
ログ出力に色をつける。

タイプ: `Boolean`<br>
デフォルト: *true*

### bail
指定した量のテストが失敗するまでテストを実行したい場合は、bail を使用します（デフォルトは0です - bail はせずに、すべてのテストを実行します）。

タイプ: `Number`<br>
デフォルト: *0* (don't bail, run all tests)

### screenshotPath
Seleniumドライバがクラッシュした場合、指定されたパスにスクリーンショットを保存します。

タイプ: `String`|`null`<br>
デフォルト: *null*

### screenshotOnReject
Seleniumドライバがクラッシュした場合、エラーに現在のページのスクリーンショットを添付します。スクリーンショット取得時のタイムアウトと再試行回数を設定するオブジェクトを指定できます。

タイプ: `Boolean`|`Object`<br>
デフォルト: *false*

**注:** エラーにスクリーンショットを添付すると、スクリーンショットを取得するための余分な時間とそれを格納するための余分なメモリが必要になります。そのため、デフォルトではパフォーマンスのために無効になっています。

**例:**

```js
// take screenshot on reject
screenshotOnReject: true

// take screenshot on reject and set some options
screenshotOnReject: {
    connectionRetryTimeout: 30000,
    connectionRetryCount: 0
}
```

### waitforTimeout
すべてのwaitForXXXコマンドのデフォルトのタイムアウト。`f` は小文字ですので注意してください。

タイプ: `Number`<br>
デフォルト: *1000*

### waitforInterval
すべてのwaitForXXXコマンドのデフォルトの間隔。

タイプ: `Number`<br>
デフォルト: *500*

### queryParams
それぞれの selenium リクエストに追加するクエリ パラメータのキーバリューストア。
タイプ: `Object`<br>
デフォルト: None

**例:**

```js
queryParams: {
  specialKey: 'd2ViZHJpdmVyaW8='
}

// Selenium request would look like:
// http://127.0.0.1:4444/v1/session/a4ef025c69524902b77af5339017fd44/window/current/size?specialKey=d2ViZHJpdmVyaW8%3D
```

## debug

node のデバッグを有効にする

タイプ: `Boolean`<br>
デフォルト: *false*

## execArgv
子プロセスの起動時に指定するノード引数

タイプ: `Array of String`<br>
デフォルト: *null*

## Setup [Babel](https://babeljs.io/) to write tests using next generation JavaScript
## 次世代のJavaScriptを使ってテストを書くために [Babel](https://babeljs.io/) をセットアップする

**注：これらの手順はBabel 6用です。Babel 5を使用することはお勧めしません。**

まず、babel の依存関係をインストールします。

```
npm install --save-dev babel-register babel-preset-es2015
```

wdio テストランナーを使って Babelを設定するには、複数の方法があります。Cucumber や Jasmine のテストを実行している場合は、設定ファイルのbefore フックに Babel を登録するだけです。

```js
    before: function() {
        require('babel-register');
    },
```

Mocha を実行する場合は、Mocha の内部コンパイラを使用して Babel を登録できます。

```js
    mochaOpts: {
        ui: 'bdd',
        compilers: ['js:babel-register'],
        require: ['./test/helpers/common.js']
    },
```

### `.babelrc` の設定

`babel-polyfill` の使用は `webdriverio` では推奨されません。
そのような機能が必要な場合はかわりに
[`babel-runtime`](https://babeljs.io/docs/plugins/transform-runtime/)
を使います。
```
npm install --save-dev babel-plugin-transform-runtime babel-runtime
```
そして、`.babelrc` に次のように設定します。
```json
{
  "presets": ["es2015"],
  "plugins": [
    ["transform-runtime", {
      "polyfill": false
    }]
  ]
}
```

`babel-preset-es2015`
のかわりに、
`babel-preset-es2015-nodeX`
(`X` は node のメジャーバージョン) を使うと、
ジェネレータなどの不要なポリフィルを避ける事ができます。

```
npm install --save-dev babel-preset-es2015-node6
```
```json
{
  "presets": ["es2015-node6"],
  "plugins": [
    ["transform-runtime", {
      "polyfill": false,
      "regenerator": false
    }]
  ]
}
```

## [TypeScript](http://www.typescriptlang.org/)のセットアップ

Babelの設定と同様に、TypeScript を登録して、設定ファイルの befor フックで.tsファイルをコンパイルすることができます。インストールされたdevDependency として [ts-node](https://github.com/TypeStrong/ts-node) が必要です。

```js
    before(function() {
        require('ts-node/register');
    }),
```

Mocha の場合も同じです。

```js
    mochaOpts: {
        ui: 'bdd',
        compilers: ['ts:ts-node/register'],
        requires: ['./test/helpers/common.js']
    },
```
