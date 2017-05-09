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

WebdriverIO コンフィグの services セクションに `visual-regression` を追加し、 `visualRegression` に希望する比較ストラテジーを定義して、wdio-visual-regression-service を設定します。

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
  すべてのスクリーンショットをスクリーン方向を変えて撮影します (例えば、レスポンシブデザインテストの場合に使います)

### 比較方法
wdio-visual-regression-serviceでは、スクリーンショットのさまざまな比較方法が使用できます。

#### VisualRegressionCompare.LocalCompare

名前の通り、LocalCompare はコンピュータ上のスクリーンショットをローカルにキャプチャし、以前の実行と比較します。

オブジェクトとしてコンストラクタに次のオプションを渡すことができます。

* **referenceName** `Function` <br>
  リファレンスとなるスクリーンショットのファイル名を返す関数を渡します。関数は、第1パラメータに、コマンドに関するすべての関連情報を含む *context* オブジェクトを受け取ります。

* **screenshotName** `Function` <br>
  現在のスクリーンショットのファイル名を返す関数を渡します。関数は、第1パラメータに、コマンドに関するすべての関連情報を含む *context* オブジェクトを受け取ります。

* **diffName** `Function` <br>
  diff スクリーンショットのファイル名を返す関数を渡します。 関数は、第1パラメータに、コマンドに関するすべての関連情報を含む *context* オブジェクトを受け取ります。


* **misMatchTolerance** `Number`  ( default: 0.01 ) <br>
  2つの画像を同一と見なすミスマッチの度合いを定義する0〜100の数値です。この値を大きくするとテストカバレッジが低下します。

現在のテスト名に依存するスクリーンショットファイル名を生成する例については、[Configuration](#configuration) のサンプルコードをご覧ください。

#### VisualRegressionCompare.SaveScreenshot

このメソッドは、`VisualRegressionCompare.LocalCompare` から、スクリーンショットだけをキャプチャする機能を取り出したものです。
前のスクリーンショットとの比較をせずに、リファレンススクリーンショットを作成して上書きするだけの場合には非常に便利です。

コンストラクタに次のオプションのオブジェクトを渡すことができます。

* **screenshotName** `Function` <br>
  現在のスクリーンショットのファイル名を返す関数を渡します。関数は、第1パラメータに、コマンドに関するすべての関連情報を含む *context* オブジェクトを受け取ります。

## 使用法

wdio-visual-regression-service は、WebdriverIO インスタンスを強化して、次のコマンドを使用できるようにします。

* `browser.checkViewport([{options}]);`
* `browser.checkDocument([{options}]);`
* `browser.checkElement(elementSelector, [{options}]);`

これらはすべて、さまざまな場面でスクリーンショットをキャプチャしたり、関連性のない部分 (コンテンツなど) を除外するのに役立つオプションを提供します。次のオプションを使用できます。

* **exclude** `String[]|Object[]` (**未実装です**)<br>
  スクリーンショットの頻繁に変化する部分を除外します。1つまたは複数の要素をクエリするさまざまな種類の[WebdriverIO の selector](http://webdriver.io/guide/usage/selectors.html) を渡すことができます。また、矩形または多角形を表すx/y値を定義することもできます。

* **hide** `String[]`<br>
  さまざまな種類の[WebdriverIO の selector](http://webdriver.io/guide/usage/selectors.html) によってクエリされるすべての要素を非表示にします (`visibility: hidden` を使います)

* **remove** `String[]`<br>
  さまざまな種類の[WebdriverIO の selector](http://webdriver.io/guide/usage/selectors.html) によってクエリされたすべての要素を削除します (`display: none` を使います)

* **widths** `Number[]` (**desktop only**)<br>
  このコマンドのグローバルの *widths* の値を上書きします。すべてのスクリーンショットをスクリーン幅を変えて撮影します (例えば、レスポンシブデザインテストの場合に使います)

* **orientations** `String[] {landscape, portrait}` (**mobile only**)<br>
  このコマンドのグローバルの *orientations* 値を上書きします。すべてのスクリーンショットをスクリーン方向を変えて撮影します (例えば、レスポンシブデザインテストの場合に使います)

* **misMatchTolerance** `Number` <br>
  このコマンドのグローバルの *misMatchTolerance* 値を上書きします。2つの画像を同一と見なすミスマッチの度合いを定義する0〜100の数値です。この値を大きくするとテストカバレッジが低下します。

* **viewportChangePause**  `Number` <br>
  このコマンドのグローバルの `viewportChangePause` 値を上書きします。ビューポートが変更されてから x ミリ秒待機します。

### ライセンス

MIT
