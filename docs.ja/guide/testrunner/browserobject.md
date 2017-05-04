name: The Browser Object
category: testrunner
tags: guide
index: 3
title: WebdriverIO - The Browser Object
---

Browser オブジェクト
==================

wdio テストランナーを使用する場合は、グローバルの `browser` オブジェクトを介して webdriver インスタンスにアクセスできます。セッションはテストランナーによって初期化されるので、 [`init`](/api/protocol/init.html) コマンドを呼び出す必要はありません。セッションを終了する場合も同じです。これもテストランナープロセスによって行われます。

browser オブジェクトには、 [api](/api.html) のすべてのコマンドの他に、テスト実行中に参考になる情報がいくつかあります。

### 必要な機能を取得する

```js
console.log(browser.desiredCapabilities);
/**
 * outputs:
 * {
       javascriptEnabled: true,
       locationContextEnabled: true,
       handlesAlerts: true,
       rotatable: true,
       browserName: 'chrome',
       loggingPrefs: { browser: 'ALL', driver: 'ALL' }
   }
 */
```

### wdio のコンフィグ オプションを取得する

```js
// wdio.conf.js
exports.config = {
    // ...
    foobar: true,
    // ...
}
```

```js
console.log(browser.options);
/**
 * outputs:
 * {
        port: 4444,
        protocol: 'http',
        waitforTimeout: 10000,
        waitforInterval: 250,
        coloredLogs: true,
        logLevel: 'verbose',
        baseUrl: 'http://localhost',
        connectionRetryTimeout: 90000,
        connectionRetryCount: 3,
        sync: true,
        specs: [ 'err.js' ],
        foobar: true, // <-- custom option
        // ...
 */
```

### 機能がモバイル デバイスかどうかを確認する

```js
var client = require('webdriverio').remote({
    desiredCapabilities: {
    	platformName: 'iOS',
        app: 'net.company.SafariLauncher',
        udid: '123123123123abc',
        deviceName: 'iPhone',
    }
});

console.log(client.isMobile); // outputs: true
console.log(client.isIOS); // outputs: true
console.log(client.isAndroid); // outputs: false
```

### 結果をログする

```js
browser.logger.info('some random logging');
```

Loggerクラスの詳細については、GitHubの[Logger.js](https://github.com/webdriverio/webdriverio/blob/master/lib/utils/Logger.js)を参照してください。
