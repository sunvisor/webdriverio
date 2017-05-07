name: dot
category: reporters
tags: guide
index: 0
title: WebdriverIO - Dot Reporter
---

Dot レポーター
============

dot は、WDIO テストランナーのデフォルトのレポーターです。そのため、WebdriverIO の依存関係内にあり、ダウンロードする必要はありません。dot レポーターを使用するには、`reporters` 配列に'dot'を追加するだけです。

```js
// wdio.conf.js
module.exports = {
  // ...
  reporters: ['dot'],
  // ...
};
```

dot レポーターは、各テスト スペックごとにドットを印刷します。マシンで色が有効になっている場合は、ドットに3種類の色が表示されます。黄色い点は、少なくとも1つのブラウザがそのスペックを実行したことを意味します。緑色の点は、すべてのブラウザがそのスペックをパスし、赤色の点が少なくとも1つのブラウザがそのスペックを満たしていないことを意味します。

![Dot Reporter](http://webdriver.io/images/dot.png "Dot Reporter")
