name: phantomjs
category: services
tags: guide
index: 6
title: WebdriverIO - PhantomJS Service
---

PhantomJS サービス
==================

[このサービス](https://github.com/cognitom/wdio-phantomjs-service)は、WDIO テストランナーによるテストの実行時に PhantomJS をシームレスに実行するのに役立ちます。これは、[phantomjs-prebuilt](https://www.npmjs.com/package/phantomjs-prebuilt) NPMパッケージを使用します。

## インストール

NPM から

```bash
npm install --save-dev wdio-phantomjs-service
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください。

## 設定

このサービスを使うには `phantomjs` を services 配列に追加します。

```js
// wdio.conf.js
export.config = {
  // ...
  services: ['phantomjs'],
  // ...
};
```

## サンプル

サンプルは[こちら](https://github.com/cognitom/webdriverio-examples/tree/master/wdio-wo-local-selenium)にあります。
