name: Appium
category: services
tags: guide
index: 3
title: WebdriverIO - Appium Service
---

Appium 用 WebdriverIO サービス
===============================

Appiumの[WebdriverIO](http://webdriver.io/) サービスプラグイン。このサービスをインストールすると、Appiumを手動で実行する必要はありません。

## インストール

最も簡単な方法は、 `wdio-appium-service` を `package.json` のdevDependency に追加することです。

```json
{
  "devDependencies": {
    "wdio-appium-service": "~0.2.2"
  }
}
```

簡単に次のようにもできます。

```bash
npm install wdio-appium-service --save-dev
```

## 使用法

このパッケージをサービスプラグインとして登録し、 [wdio.conf](http://webdriver.io/guide/getstarted/configuration.html) にコマンドライン引数を指定してください。コマンドには`'appium'` が使用されます。 `command` キーが指定されている場合は、そのキーが使用されます。

```javascript
{
  ... // Other config

  services: ['appium'],

  appium: {
    args: {
      address: '127.0.0.1',
      commandTimeout: '7200',
      sessionOverride: true,
      debugLogSpacing: true,
      platformVersion: '9.1',
      platformName: 'iOS',
      showIosLog: true,
      deviceName: 'iPhone 6',
      nativeInstrumentsLib: true,
      isolateSimDevice: true,
      app: APP_PATH
    }
  }
}
```

`args` には、lowerCamel でキーを指定できます。その値はその値として解釈されます。`value` が `boolean` の場合、`true` はキーを指定したこと、`false` は指定しないことを意味します。

例えば、`platformVersion: '9.1'` は `--platform-version=9.1`に、`sessionOverride: true` は `--session-override`　に、`showIosLog: false` は、何も指定されません。
