name: Timeouts
category: testrunner
tags: guide
index: 5
title: WebdriverIO - Timeouts
---

タイムアウト
========

WebdriverIO の各コマンドは、Selenium サーバー (または[Sauce Labs](https://saucelabs.com/) のようなクラウドサービス) にリクエストを送信する非同期操作であり、そのレスポンスには処理が完了したのか失敗したのかの結果が含まれます。したがって、時間は全体のテストプロセスにおいて重要な要素です。特定のアクションが異なるアクションの状態に依存する場合、それらが正しい順序で実行されることを確認する必要があります。タイムアウトは、これらの問題に対処する際に重要な役割を果たします。

## Selenium のタイムアウト

### セッション スクリプトのタイムアウト

セッションには、非同期スクリプトの実行を待つ時間を指定する、セッションスクリプト関連のタイムアウトがあります。指定されていない場合は 30秒です。このタイムアウトは次のように設定できます。

```js
browser.timeouts('script', 60000);
browser.executeAsync(function (done) {
    console.log('this should not fail');
    setTimeout(done, 59000);
});
```

### セッションページのロード タイムアウト

セッションには、ページロードが完了するのを待つ時間を指定する、セッションページのロード関連のタイムアウトがあります。指定されていない場合は 300,000ミリ秒です。このタイムアウトは次のように設定できます。


```js
browser.timeouts('pageLoad', 10000);
```

> `pageLoad` キーワードは公式のWebDriver [仕様](https://www.w3.org/TR/webdriver/#set-timeouts)の一部ですが、ブラウザで[サポート](https://github.com/seleniumhq/selenium-google-code-issue-archive/issues/687)されていない可能性があります (以前の名前は `page load`) 。

### セッションの暗黙のタイムアウト

セッションには、セッションの暗黙的待機に関連するタイムアウトがあります。
これは [`element`](/api/protocol/element.html) や [`elements`](/api/protocol/elements.html) コマンドを使ってエレメントを配置する際に、暗黙のエレメント配置ストラテジーのための待ち時間を指定します。
指定されない場合、0ミリ秒です。このタイムアウトは次のように設定できます。

```js
browser.timeouts('implicit', 5000);
```

## WebdriverIO 関連のタイムアウト

### WaitForXXX タイムアウト

WebdriverIO は、要素が特定の状態 (enabled、visible、existing など) に達するのを待つ複数のコマンドを提供します。これらのコマンドは、セレクタ引数と、その要素がその状態に達するのをインスタンスが待つ時間を宣言するタイムアウト値を受け取ります。`waitforTimeout` オプションを使用すると、すべてのwaitForコマンドのグローバルタイムアウトを設定できるので、同じタイムアウトを繰り返し設定する必要はありません。`waitfor` の `f` は小文字ですので注意してください。

```js
// wdio.conf.js
exports.config = {
    // ...
    waitforTimeout: 5000,
    // ...
};
```

テストでは次のようにできます。

```js
var myElem = browser.element('#myElem');
myElem.waitForVisible();

// 以下と同等です
browser.waitForVisible('#myElem');

// 以下と同等です
browser.waitForVisible('#myElem', 5000);
```

## フレームワーク関連のタイムアウト

また、WebdriverIO で使用するテストフレームワークは、すべてが非同期であるため、タイムアウトに対処する必要があります。何か問題が生じた場合にテストプロセスが停止しないようにします。デフォルトでは、タイムアウトは10秒に設定されています。つまり、1回のテストではそれ以上の時間をかけられません。Mocha の1回のテストは次のようになります：

```js
it('should login into the application', function () {
    browser.url('/login');

    var form = browser.element('form');
    var username = browser.element('#username');
    var password = browser.element('#password');

    username.setValue('userXY');
    password.setValue('******');
    form.submit();

    expect(browser.getTitle()).to.be.equal('Admin Area');
});
```

Cucumber では、タイムアウトは1つのステップ定義に適用されます。ただし、テストにデフォルト値よりも時間がかかるため、タイムアウトを増やしたい場合は、フレームワークオプションで設定する必要があります。次のは Mocha の場合です。

```js
// wdio.conf.js
exports.config = {
    // ...
    framework: 'mocha',
    mochaOpts: {
        timeout: 20000
    },
    // ...
}
```

Jasmine 用

```js
// wdio.conf.js
exports.config = {
    // ...
    framework: 'jasmine',
    jasmineNodeOpts: {
        defaultTimeoutInterval: 20000
    },
    // ...
}
```

そして Cucumber 用

```js
// wdio.conf.js
exports.config = {
    // ...
    framework: 'cucumber',
    cucumberOpts: {
        timeout: 20000
    },
    // ...
}
```
