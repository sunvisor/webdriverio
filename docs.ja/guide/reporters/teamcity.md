name: teamcity
category: reporters
tags: guide
index: 4
title: WebdriverIO - Teamcity Reporter
---

WDIO Teamcity レポーター
======================

テスト結果をリアルタイムで表示することを可能にする WebdriverIO Teamcity レポーターは、ビルド結果ページの Tests タブでテスト情報を利用可能にします。

## インストール

```bash
npm install wdio-teamcity-reporter --save-dev
```

WebdriverIO のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください 。

## 設定

レポーターを [wdio.conf.js](http://webdriver.io/guide/testrunner/configurationfile.html) ファイルの reporters リストに追加します。

```js
exports.config = {
  // ...
  reporters: ['teamcity'],
  // ...
}
```

## リンク

- レポートメッセージに関するTeamcityのドキュメントを参照してください:    
<https://confluence.jetbrains.com/display/TCD65/Build+Script+Interaction+with+TeamCity>
