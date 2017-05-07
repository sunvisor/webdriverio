name: repl
category: usage
tags: guide
index: 9
title: WebdriverIO - REPL interface
---

REPL インターフェース
=====================

`v4.5.0` では、WebdriverIO に[REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)インターフェイスが導入され、フレームワークAPIの検出だけでなく、テストのデバッグや検査にも役立ちます。それは複数の方法で使用することができます。 まず、CLIコマンドとして使用し、コマンド行からSeleniumセッションを生成することができます。

```sh
$ wdio repl chrome
```

これにより、REPLインターフェイスで制御できるChromeブラウザが開きます。セッションを開始するには、ポート `4444` で Selenium サーバーを実行していることを確認してください。[SauceLabs](https://saucelabs.com)  (または他のクラウドベンダー) アカウントをお持ちの場合は、次の方法でクラウド内のコマンドラインでブラウザを直接実行することもできます。

```sh
$ wdio repl chrome -u $SAUCE_USERNAME -k $SAUCE_ACCESS_KEY
```

REPL セッションで使用できるオプション(`wdio --help` を参照) を適用することができます。REPL を使用する際には、いくつかの高度な機能 (例えば、`$` や `$$` 関数のようなもの) を使うことができるので、[`wdio-sync`](https://github.com/webdriverio/wdio-sync) パッケージを (フレームワークアダプタと一緒にインストールされていない場合) インストールすることを推奨します。

![WebdriverIO REPL](http://webdriver.io/images/repl.gif)

REPLを使用する別の方法は、[`debug`](/api/utility/debug.html) コマンドを使用してテストの間に実行します。実行時にブラウザを停止し、アプリケーションにジャンプしたり (例えば、開発ツール) 、コマンドラインからブラウザを制御することができます。これは、一部のコマンドが特定のアクションを期待どおりに起動しない場合に役立ちます。 REPLを使用すると、最も信頼性の高いコマンドを試すことができます。
