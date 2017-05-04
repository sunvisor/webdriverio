name: eventhandling
category: usage
tags: guide
index: 6
title: WebdriverIO - Eventhandling
---

イベント処理
=============

サポートされている関数は
`on`,`once`,`emit`,`removeListener`,`removeAllListeners`
です。

NodeJS 公式ドキュメントに記述されているとおりに動作します。重要な WebdriverIO イベントをカバーする事前定義されたイベント (`error`,`init`,`end`, `command`, `log`) があります。

## サンプル

```js
browser.on('error', function(e) {
    // will be executed everytime an error occurred
    // e.g. when element couldn't be found
    console.log(e.body.value.class);   // -> "org.openqa.selenium.NoSuchElementException"
    console.log(e.body.value.message); // -> "no such element ..."
})
```

`log()` イベントを使用して任意のデータをログに記録し、記録したりリポーターに表示させたりできます。

```js
browser
    .init()
    .emit('log', 'Before my method')
    .click('h2.subheading a')
    .emit('log', 'After my method', {more: 'data'})
    .end();
```

すべてのコマンドはチェーンできるので、コマンドをチェーンさせながら使用することができます。

```js
var cnt;

browser
    .init()
    .once('countme', function(e) {
        console.log(e.elements.length, 'elements were found');
    })
    .elements('.myElem').then(function(res) {
        cnt = res.value;
    })
    .emit('countme', cnt)
    .end();
```

リスナー機能内では、WebdriverIO コマンドやその他の非同期操作を実行できないことに注意してください。イベント処理は、特定の情報をログに記録するときに便利ですが、エラーが発生した場合にスクリーンショットを撮るなど、特定のイベントに対してアクションを実行するために使用されるとはみなされません。そうするには、wdio テストランナー設定ファイルで onError フックを使用します。
