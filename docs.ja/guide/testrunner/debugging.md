name: Debugging
category: testrunner
tags: guide
index: 8
title: WebdriverIO - Test Runner Frameworks
---

デバッグ
==========

複数のブラウザで数回のテストが発生するプロセスがある場合、デバッグはとても難しくなります。

まず、`maxInstances` を1に設定し、デバッグが必要なスペックとブラウザのみを対象として、並列性を制限する方法は非常に役立ちます。


`wdio.conf` にて
```
maxInstances: 1,
specs: ['**/myspec.spec.js'],
capabilities: [{browserName: 'firefox'}]
```

多くの場合、[`browser.debug()`](/api/utility/debug.html) を使用してテストを一時停止し、ブラウザを検査することができます。コマンドラインインターフェースもREPL モードに切り替わり、ページ上のコマンドや要素を操作することができます。REPL モードでは、ブラウザオブジェクトや、'$' や `$$` 関数にアクセスすることができます。

`browser.debug()` を使用する場合は、テストランナーのタイムアウトを長くしてテストランナーがテストに失敗するのを防ぐ必要があります。例えば：

`wdio.conf` にて
```
jasmineNodeOpts: {
  defaultTimeoutInterval: (24 * 60 * 60 * 1000);
}
```

他のフレームワークを使用してこうする方法の詳細については、 [timeouts](/guide/testrunner/timeouts.html)を参照してください。

## ファイルを見る

`v4.6.0` では、WebdriverIO は更新されたスペックを実行するのに役立つ watch 引数が導入されました。これを有効にするには、以下のように watch フラグを指定して wdio コマンドを実行してください：

```sh
wdio wdio.conf.js --watch
```

コンフィグで定義されている対象の Selenium セッションを初期化し、specs オプションで定義されたファイルが変更されるまで待ちます。これは、ローカルグリッドでも[SauceLabs](https://saucelabs.com/) のようなクラウドサービスでテストを実行しても機能します。

## ノードインスペクタ

**注: ノードv6.3以上を使用している場合は、代わりにノードの組み込みデバッガを使用する必要があります。[下記参照](#node_debugger)**

より広範囲なデバッグ環境を実現するには、デバッグフラグを有効にして、デバッガポートを開いたテストランナープロセスを開始します。

これにより、node-inspector をアタッチし、`debugger` でテスト実行を一時停止することができます。それぞれの子プロセスには 5859 から始まる新しいデバッグポートが割り当てられます。

この機能は、wdio.conf の `debug` フラグを立てることで有効にできます。

```
{
  debug: true
}
```

有効になると、 `debugger` ステートメントでテストが一時停止します。続行するには、デバッガを接続する必要があります。

`node-inspector` をまだインストールしていない場合は、次のようにインストールします。

```
npm install -g node-inspector
```

そして、次の方法でプロセスにアタッチします。

```
node-inspector --debug-port 5859 --no-preload
```

`no-preload` オプションは、必要なだけのソースファイルをロードすることを防ぎます。これは、プロジェクトに多数の node\_modules が含まれている場合にパフォーマンスを大幅に向上させますが、ソースをナビゲートしてデバッガを接続した後でブレークポイントを追加する必要がある場合は、このオプションは指定しません。

## chrome-devtoolsによるノードビルトインデバッグ<a id="node_debugger"></a>

Chromeの開発者ツールでのデバッグは、node-inspecto rの代わりになるように思えます。次は node-inspecto の github の READMEからです。

> バージョン6.3 以降、Node.js は開発者ツール ベースのデバッガを提供しています。これは主にノードインスペクタを非推奨にしています。ビルトインデバッガは、V8 / Chromium チームによって直接開発され、ノードインスペクタでは実装が難しい高度な機能 (長いスタックトレース、非同期のスタックトレースなど) を提供します。

動作させるには、次のように、テストを実行するノードプロセスに `--inspect` フラグを渡す必要があります。

`wdio.conf`にて

```
execArgv: ['--inspect']
```

コンソールに次のようなメッセージが表示されます。

```
Debugger listening on port 9229.
Warning: This is an experimental feature and could change at any time.
To start debugging, open the following URL in Chrome:
    chrome-devtools://devtools/remote/serve_file/@60cd6e859b9f557d2312f5bf532...
```

そのURLを開くと、デバッガが接続されます。

テストは `debugger` ステートメントで一時停止しますが、開発者ツールがが開かれ、デバッガが接続されている場合にのみ有効です。
これはちょっと困ったことなので、スクリプトの最初の文を `--debug-brk` で打ち切ることもできます。
これにより、デバッガをアタッチする時間が確保されます。

```
execArgv: ['--inspect','--debug-brk']
```

実行が終了すると、開発者ツールが閉じられるまでテストは終了しません。それは自分で行う必要があります。

## 動的構成

`wdio.conf` には JavaScript を含めることができます。タイムアウト値を1日に変更するのをそのままにはしたくないでしょう。そんな場合は、環境変数を使用してコマンドラインからこれらの設定を変更すると便利な場合があります。これは、構成を動的に変更するために使用できます。

```
var debug = process.env.DEBUG;
var defaultCapabilities = ...;
var defaultTimeoutInterval = ...;
var defaultSpecs = ...;


exports.config = {
debug: debug,
maxInstances: debug ? 1 : 100,
capabilities: debug ? [{browserName: 'chrome'}] : defaultCapabilities,
specs:  process.env.SPEC ? [process.env.SPEC] : defaultSepcs,
jasmineNodeOpts: {
  defaultTimeoutInterval: debug ? (24 * 60 * 60 * 1000) : defaultTimeoutInterval
}

```

```
DEBUG=true SPEC=myspec ./node_modules/.bin/wdio wdio.conf
```

## Atomによる動的レプリケーション

あなたが[Atom](https://atom.io/)ハッカーの場合は、[@kurtharriger](https://github.com/kurtharriger) による [wdio-repl](https://github.com/kurtharriger/wdio-repl) を試すことができます。これは動的な repl で、Atomで単一のコード行を実行できるようにします。デモを見るには[この](https://www.youtube.com/watch?v=kdM05ChhLQE) Youtubeのビデオをご覧ください。
