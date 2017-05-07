name: customreporter
category: reporters
tags: guide
index: 8
title: WebdriverIO - Custom Reporter
---

カスタム レポーター
==================

あなたのニーズに合った wdio テストランナーのための独自のカスタムレポーターを書くことができます。必要なのは、Eventemitter オブジェクトを継承する node モジュールを作成して、テストからメッセージを受信できるようにすることだけです。基本的な構成は次のようになります。

```js
var util = require('util'),
    events = require('events');

var CustomReporter = function(options) {
    // you can access reporter options via
    // `options.reporterOptions`
    // ...
};

/**
 * EventEmitter を継承
 */
util.inherits(CustomReporter, events.EventEmitter);

/**
 * Custom Reporter を公開する
 */
exports = module.exports = CustomReporter;
```

このレポーターを使用するために今行うべき唯一のことは、それを reporter プロパティに割り当てることです。したがって、wdio.conf.js ファイルは次のようになります。

```js
var CustomReporter = require('./reporter/my.custom.reporter');

exports.config = {
    // ...
    reporters: [CustomReporter],
    // ...
};
```

## Events

テスト中にトリガされるイベントに対してイベントハンドラを登録できます。これらのハンドラはすべてペイロードを受信し、そこには現在の状態や進行状況に関する有益な情報を含みます。これらのペイロードオブジェクトの構造はイベントに依存し、フレームワーク（Mocha、Jasmine、Cucumber）全体で統一されています。カスタムレポーターを実装したら、すべてのフレームワークで機能するはずです。次のリストには、イベントハンドラを登録できるイベントがすべて含まれています。

```txt
'start'
'suite:start'
'hook:start'
'hook:end'
'test:start'
'test:end'
'test:pass'
'test:fail'
'test:pending'
'suite:end'
'end'
```

イベント名はかなり自明です。特定のイベントで何かを出力するには、ノードの [EventEmitter](https://nodejs.org/api/events.html) からのユーティリティ関数を使用してイベントハンドラを登録します。

```js
this.on('test:pass', function() {
    console.log('Hurray! A test has passed!');
});
```

テストの実行を決して延期することはできません。すべてのイベント ハンドラは同期ルーチンを実行する必要があります。そうしないと競合状態になります。各イベントのイベント名を出力するカスタム レポータのサンプルがある[サンプル セクション](https://github.com/webdriverio/webdriverio/tree/master/examples/wdio)を確認してください。コミュニティに役立つカスタムレポーターを実装している場合は、プルリクエストをすることを躊躇しないでください。レポーターを公開できるようにします。
