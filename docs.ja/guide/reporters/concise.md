name: concise
category: reporters
tags: guide
index: 6
title: WebdriverIO - Concise Reporter
---


WDIO Concise レポーター
=======================

WebdriverIOの consise レポーター。このプロジェクトは [WDIO-Json-Reporter](https://github.com/fijijavis/wdio-json-reporter) から派生したものです。

![WDIO Concise Reporter error](https://github.com/FloValence/wdio-concise-reporter/blob/master/example_error.png?raw=true)
![WDIO Concise Reporter success](https://github.com/FloValence/wdio-concise-reporter/blob/master/example_success.png?raw=true)

## インストール


最も簡単な方法は、 `wdio-json-reporter` を `package.json` の `devDependency` に記述することです。

```json
{
  "devDependencies": {
    "wdio-concise-reporter": "~0.0.1"
  }
}
```

次のように簡単に実行することができます

```bash
npm install wdio-concise-reporter --save-dev
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください 。

## 設定

次のコードは、デフォルトのwdioテストランナー設定を示しています。レポーターとして `'concise'` を配列に追加するだけです。

```js
// wdio.conf.js
module.exports = {
  // ...
  reporters: ['concise'],
  // ...
};
```
