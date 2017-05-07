name: selenium-standalone
category: services
tags: guide
index: 4
title: WebdriverIO - Selenium Standalone Service
---

Selenium Standalone サービス
============================

Seleniumサーバーの処理は、実際の WebdriverIO プロジェクトの範囲外になります。このサービスは、[WDIO テストランナー](http://webdriver.io/guide/testrunner/gettingstarted.html)によるテストの実行時に、Selenium をシームレスに実行するのに役立ちます。スタンドアロン サーバーと必要なすべてのドライバを自動的にセットアップする、よく知られている[selenium-standalone](https://www.npmjs.com/package/selenium-standalone) NPMパッケージを使用します。

## インストール

最も簡単な方法は、`package.json` の devDependencies に `wdio-selenium-standalone-service` を追加することです。

```json
{
  "devDependencies": {
    "wdio-selenium-standalone-service": "~0.1"
  }
}
```

簡単に次のようにもできます。

```bash
npm install wdio-selenium-standalone-service --save-dev
```

WebdriverIOのインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください。

## 設定

デフォルトでは、Google Chrome、Firefox、PhantomJS は、ホストシステムにインストールされている場合に利用できます。サービスを使用するには、services 配列に `selenium-standalone` を追加します。

```js
// wdio.conf.js
export.config = {
  // ...
  services: ['selenium-standalone'],
  // ...
};
```

## オプション

### seleniumLogs
Seleniumサーバーからのログを保存するパス。

Type: `String`

### seleniumArgs
Selenium サーバーの引数の配列。`child_process.spawn` に直接渡されます。

Type: `String[]`<br>
Default: `[]`
