name: customcommands
category: usage
tags: guide
index: 1
title: WebdriverIO - Custom Commands
---

カスタム コマンド
===============

独自のコマンドセットでブラウザ インスタンスを拡張したい場合は、browser オブジェクトの `addCommand` メソッドを使用できます。コマンドは、スペックと同様に、同期（デフォルト）で記述することも、（スタンドアロンモードでWebdriverIOを使用する場合と同様に）非同期で記述することもできます。次の例は、現在のURLとタイトルを1つの結果として返す新しいコマンドを同期コマンドを使用して追加する方法を示しています。

```js
browser.addCommand("getUrlAndTitle", function (customVar) {
    return {
        url: this.getUrl(),
        title: this.getTitle(),
        customVar: customVar
    };
});
```

カスタムコマンドを使えば、頻繁に使用される特定のコマンドシーケンスをまとめて、便利な単一のコマンド呼び出しですませることができます。カスタムコマンドは、テストスイートのどの時点でも定義できます。最初にコマンドを使用する前にコマンドが定義されていることを確認してください（wdio.conf.js の before フックで作成するのが良いかもしれません）。また注意すべきことは、すべての WebdriverIO コマンドのようなカスタムコマンドは、テストフックまたはブロック内でのみ呼び出すことができるという点です。スペックファイルでは、次のように使用することができます。

```js
it('should use my custom command', function () {
    browser.url('http://www.github.com');
    var result = browser.getUrlAndTitle('foobar');

    assert.strictEqual(result.url, 'https://github.com/');
    assert.strictEqual(result.title, 'GitHub · Where software is built');
    assert.strictEqual(result.customVar, 'foobar');
});
```

前に述べたように、古き良き Promise の構文を使ってコマンドを定義することができます。これは、Promise をサポートするサードパーティライブラリを使用して作業する場合や、両方のコマンドを同時に実行したい場合に理にかなっています。次に非同期の例を示します。カスタム コマンド コールバックには、`async` という関数名があります。

```js
client.addCommand("getUrlAndTitle", function async () {
    return Promise.all([
        this.getUrl(),
        this.getTitle()
    ]);
});
```

## サードパーティのライブラリを統合する

Promise をサポートする外部ライブラリ（データベース呼び出しなど）を使用している場合、カスタムコマンド内に対象のAPIメソッドをラップするのが簡単な方法です。


```js
browser.addCommand('doExternalJob', function async (params) {
    return externalLib.command(params);
});
```

次に、wdio テスト スペックで同期的に使えます。

```js
it('execute external library in a sync way', function() {
    browser.url('...');
    browser.doExternalJob('someParam');
    console.log(browser.getTitle()); // returns some title
});
```

カスタムコマンドの戻り値は、あなたが返す Promise の戻り値であることに注意してください。また、スタンドアロンモードでの同期コマンドはサポートされていないため、常にPromise を使用して非同期コマンドを処理する必要があります。

既存のコマンドを上書きしようとすると、デフォルトでは WebdriverIO がエラーをスローします。 この動作を回避するには、 `addCommand` 関数の3番目のパラメータに `true` を渡します。
