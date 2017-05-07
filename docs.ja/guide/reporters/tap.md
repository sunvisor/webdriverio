name: tap
category: reporters
tags: guide
index: 7
title: WebdriverIO - TAP Reporter
---

WDIO TAP レポーター
===================

WebdriverIO TAP レポーターは、テスト結果を [TAP 13フォーマット](https://testanything.org/tap-version-13-specification.html)で出力することができるため、出力は[どのTAPレポーター](https://github.com/sindresorhus/awesome-tap#reporters)とも互換性があります。

## インストール

```bash
npm install wdio-tap-reporter --save-dev
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください 。

## 設定

レポーターを [wdio.conf.js](http://webdriver.io/guide/testrunner/configurationfile.html) ファイルの reporters リストに追加します。

```js
exports.config = {
  // ...
  reporters: ['tap'],
  // ...
}
```

## リンク

- TAP レポーターの詳細な説明は
 <https://github.com/LKay/wdio-tap-reporter>
 を参照してください。
