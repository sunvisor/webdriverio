name: Visual Regression
category: services
tags: guide
index: 8
title: WebdriverIO - Visual Regression Service
---

Visual Regression サービス
==========================

> WebDriverIO による Visual Regression テスト。

## インストール

wdio-visual-regression-service はスクリーンショットをキャプチャするために [wdio-screenshot](https://github.com/zinserjan/wdio-screenshot) を使います。

wdio-visual-regression-service はいつもの通り NPM でインストールできます。

```sh
$ npm install wdio-visual-regression-service --save-dev
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください。

wdio-visual-regressionサービスを使用するリポジトリの例を[ここに](https://github.com/zinserjan/webdriverio-example)示します。

## 設定

Setup wdio-visual-regression-service by adding `visual-regression` to the service section of your WebdriverIO config and define your desired comparison strategy in `visualRegression`.
WebdriverIO コンフィグの services セクションに `visual-regression` を追加し、`visualRegression` に希望する比較ストラテジーを定義して、wdio-visual-regression-service を設定します。

```js
// wdio.conf.js

var path = require('path');
var VisualRegressionCompare = require('wdio-visual-regression-service/compare');

function getScreenshotName(basePath) {
  return function(context) {
    var type = context.type;
    var testName = context.test.title;
    var browserVersion = parseInt(context.browser.version, 10);
    var browserName = context.browser.name;
    var browserWidth = context.meta.width;

    return path.join(basePath, `${testName}_${type}_${browserName}_v${browserVersion}_${browserWidth}.png`);
  };
}

exports.config = {
  // ...
  services: [
    'visual-regression',
  ],
  visualRegression: {
    compare: new VisualRegressionCompare.LocalCompare({
      referenceName: getScreenshotName(path.join(process.cwd(), 'screenshots/reference')),
      screenshotName: getScreenshotName(path.join(process.cwd(), 'screenshots/screen')),
      diffName: getScreenshotName(path.join(process.cwd(), 'screenshots/diff')),
      misMatchTolerance: 0.01,
    }),
    viewportChangePause: 300,
    widths: [320, 480, 640, 1024],
    orientations: ['landscape', 'portrait'],
  },
  // ...
};
```

### オプション

`visualRegression` キーの下に、次の構造を持つコンフィグ オブジェクトを渡すことができます。

* **compare** `Object` <br>
スクリーンショットを比較する方法。[Compare Methods](#compare-methods) を参照。

* **viewportChangePause**  `Number`  ( default: 100 ) <br>
  ビューポートが変更されてから x ミリ秒待機します。ブラウザが再描画するにはしばらく時間がかかります。これによりレンダリングの問題が発生し、実行ごとに一貫性のない結果が生成される可能性があります。

* **widths** `Number[]`  ( default: *[current-width]* ) (**デスクトップのみ**)<br>
  すべてのスクリーンショットをスクリーン幅を変えて撮影します (例えば、レスポンシブデザインテストの場合に使います)

* **orientations** `String[] {landscape, portrait}`  ( default: *[current-orientation]* ) (**モバイルのみ**)<br>
  all screenshots will be taken in different screen orientations (e.g. for responsive design tests)
  すべてのスクリーンショットをスクリーン方向を変えて撮影します (例えば、レスポンシブデザインテストの場合に使います)

### 比較方法
wdio-visual-regression-serviceでは、スクリーンショットのさまざまな比較方法が使用できます。

#### VisualRegressionCompare.LocalCompare

名前の通り、LocalCompare はコンピュータ上のスクリーンショットをローカルにキャプチャし、以前の実行と比較します。

オブジェクトとしてコンストラクタに次のオプションを渡すことができます。

* **referenceName** `Function` <br>
  リファレンスとなるスクリーンショットのファイル名を返す関数を渡します。関数は、第1パラメータに、コマンドに関するすべての関連情報を含む*コンテキスト*オブジェクトを受け取ります。

* **screenshotName** `Function` <br>
pass in a function that returns the filename for the current screenshot. Function receives a *context* object as first parameter with all relevant information about the command.
  現在のスクリーンショットのファイル名を返す関数を渡します。関数は、第1パラメータに、コマンドに関するすべての関連情報を含む*コンテキスト*オブジェクトを受け取ります。

* **diffName** `Function` <br>
pass in a function that returns the filename for the diff screenshot. Function receives a *context* object as first parameter with all relevant information about the command.
diffスクリーンショットのファイル名を返す関数を渡します。 関数は、コマンドに関するすべての関連情報を含む第1パラメータとしてコンテキストオブジェクトを受け取ります。


* **misMatchTolerance** `Number`  ( default: 0.01 ) <br>
number between 0 and 100 that defines the degree of mismatch to consider two images as identical, increasing this value will decrease test coverage.
2つの画像を同一と見なすミスマッチの程度を定義する0〜100の数値ですが、この値を大きくするとテストカバレッジが低下します。

For an example of generating screenshot filesnames dependent on the current test name, have a look at the sample code of [Configuration](#configuration).
現在のテスト名に依存するスクリーンショットファイル名を生成する例については、Configurationのサンプルコードを見てください。

#### VisualRegressionCompare.SaveScreenshot
This method is a stripped variant of `VisualRegressionCompare.LocalCompare` to capture only screenshots. This is quite useful when you just want to create reference screenshots and overwrite the previous one without diffing.
このメソッドは、 VisualRegressionCompare.LocalCompareショットのみをキャプチャするVisualRegressionCompare.LocalCompareのストリップされた変形です。 リファレンススクリーンショットを作成し、前のスクリーンショットを差分なしで上書きするだけの場合には非常に便利です。

You can pass the following options to it's constructor as object:
オブジェクトとしてコンストラクタに次のオプションを渡すことができます：

* **screenshotName** `Function` <br>
pass in a function that returns the filename for the current screenshot. Function receives a *context* object as first parameter with all relevant information about the command.
現在のスクリーンショットのファイル名を返す関数を渡します。 関数は、コマンドに関するすべての関連情報を含む第1パラメータとしてコンテキストオブジェクトを受け取ります。

## Usage
wdio-visual-regression-service enhances an WebdriverIO instance with the following commands:
wdio-visual-regression-serviceは、次のコマンドを使用してWebdriverIOインスタンスを強化します。

* `browser.checkViewport([{options}]);`
* `browser.checkDocument([{options}]);`
* `browser.checkElement(elementSelector, [{options}]);`


All of these provide options that will help you to capture screenshots in different dimensions or to exclude unrelevant parts (e.g. content). The following options are
available:
これらはすべて、さまざまな次元でスクリーンショットをキャプチャしたり、関連性のない部分 (コンテンツなど) を除外するのに役立つオプションを提供します。 次のオプションを使用できます。

* **exclude** `String[]|Object[]` (**まだ実装されていませんnot yet implemented**)<br>
  exclude frequently changing parts of your screenshot, you can either pass all kinds of different [WebdriverIO selector strategies](http://webdriver.io/guide/usage/selectors.html)
  that queries one or multiple elements or you can define x and y values which stretch a rectangle or polygon

スクリーンショットの頻繁に変化する部分を除外すると、1つまたは複数の要素をクエリするさまざまな種類のWebdriverIOセレクタ戦略を渡すことができます。また、矩形またはポリゴンを引き伸ばすxおよびy値を定義することもできます

* **hide** `String[]`<br>
  hides all elements queried by all kinds of different [WebdriverIO selector strategies](http://webdriver.io/guide/usage/selectors.html) (via `visibility: hidden`)
さまざまな種類のWebdriverIOセレクター戦略によって照会されるすべての要素を非表示にします ( visibility: hidden ) 

* **remove** `String[]`<br>
  removes all elements queried by all kinds of different [WebdriverIO selector strategies](http://webdriver.io/guide/usage/selectors.html) (via `display: none`)
さまざまな種類のWebdriverIOセレクター戦略によって照会されたすべての要素を削除します ( display: none ) 

* **widths** `Number[]` (**desktop only**)<br>
     Overrides the global *widths* value for this command. All screenshots will be taken in different screen widths (e.g. for responsive design tests)
このコマンドのグローバル幅の値を上書きします。 すべてのスクリーンショットは異なるスクリーン幅で撮影されます (例えば、レスポンシブデザインテストの場合) 

* **orientations** `String[] {landscape, portrait}` (**mobile only**)<br>
    Overrides the global *orientations* value for this command. All screenshots will be taken in different screen orientations (e.g. for responsive design tests)

このコマンドのグローバル方向値を上書きします。 すべてのスクリーンショットは、さまざまなスクリーン方向で撮影されます (例：応答性の高い設計テストの場合) 

* **misMatchTolerance** `Number` <br>
    Overrides the global *misMatchTolerance* value for this command. Pass in a number between 0 and 100 that defines the degree of mismatch to consider two images as identical,
このコマンドのグローバルmisMatchTolerance値をオーバーライドします。 2つの画像を同一とみなすために不一致の度合いを定義する0〜100の数値を渡します。

* **viewportChangePause**  `Number` <br>
    Overrides the global *viewportChangePause* value for this command. Wait x milliseconds after viewport change.

このコマンドのグローバルviewportChangePause値をオーバーライドします。 ビューポートが変更されてからxミリ秒待ってください。

### License

MIT
