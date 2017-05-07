name: spec
category: reporters
tags: guide
index: 1
title: WebdriverIO - Spec Reporter
---

Spec レポーター
============

> スペック スタイルでレポートする WebdriverIO プラグイン

![Spec Reporter](http://webdriver.io/images/spec.png "Spec Reporter")

## インストール

最も簡単な方法は、 `wdio-json-reporter` を `package.json` の `devDependency` に記述することです。

```json
{
  "devDependencies": {
    "wdio-spec-reporter": "~0.0.1"
  }
}
```

次のように簡単に実行することができます

```bash
npm install wdio-spec-reporter --save-dev
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください 。

## 設定

次のコードは、デフォルトのwdioテストランナー設定を示しています。レポーターとして `'spec'` を配列に追加するだけです。

```js
// wdio.conf.js
module.exports = {
  // ...
  reporters: ['spec'],
  // ...
};
```
