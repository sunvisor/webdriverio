name: Retry Flaky Tests
category: testrunner
tags: guide
index: 9
title: WebdriverIO - Retry Flaky Tests
---

不安定なテストのリトライ
=================

WebdriverIO テストランナーで特定のテストを再実行すると、不安定なネットワークや競合状態などにより不安定になることがあります。ただし、テストが不安定になった場合は、再実行率を増やすことはお勧めしません。

## MochaJS でのスイートの再実行

MochaJSのバージョン3以降では、すべてのテストスイート (`describe` ブロック内のすべて) を再実行できます。Mocha を使っている場合は、特定のテストブロック (`it` ブロック内のすべて) を再実行できる WebdriverIO 実装よりも、この再試行メカニズムを優先する必要があります。
ここでは、MochaJS でスイート全体を再実行する方法の例を示します。

```js
describe('retries', function() {
    // Retry all tests in this suite up to 4 times
    this.retries(4);

    beforeEach(function () {
        browser.url('http://www.yahoo.com');
    });

    it('should succeed on the 3rd try', function () {
        // Specify this test to only retry up to 2 times
        this.retries(2);
        console.log('run');
        expect(browser.isVisible('.foo')).to.eventually.be.true;
    });
});
```

## JasmineまたはMochaでの1つのテストの再実行

特定のテストブロックを再実行するには、テストブロック機能の後に最後のパラメータとして再実行回数を指定します。

```js
describe('my flaky app', function () {
    /**
     * 最大4回実行するスペック (1回の実際の実行+ 3回の再実行)
     */
    it('should rerun a test at least 3 times', function () {
        // ...
    }, 3);
});
```

The same works for hooks too:

```js
describe('my flaky app', function () {
    /**
     * 最大2回実行するフック (1回の実際の実行+ 1回の再実行)
     */
    beforeEach(function () {
        // ...
    }, 1)

    // ...
});
```

スイート全体、フックまたはテストブロックのみを再実行することは__できません__ 。これを使用するには、`v0.3.0` 以降であれば [wdio-mocha-framework](https://github.com/webdriverio/wdio-mocha-framework) アダプタをインストールするか、`v0.2.0`以降であれば[wdio-jasmine-framework](https://github.com/webdriverio/wdio-jasmine-framework) アダプタをインストールする必要があります。

## Cucumber のステップ定義を再実行する

特定のステップ定義の再実行レートを定義するには、次のように再試行オプションを適用します。

```js
module.exports = function () {
    /**
     * 最大3回実行するステップ定義（1回の実際の実行+ 2回の再実行
     */
    this.Given(/^some step definition$/, { retry: 2 }, () => {
        // ...
    })
    // ...
```

再実行は、ステップ定義ファイルでのみ定義でき、featuer ファイルでは定義できません。 これを使用するには、 v0.1.0以上で[wdio-cucumber-framework](https://github.com/webdriverio/wdio-cucumber-framework) アダプターをインストールするv0.1.0があります。
