name: gettingstarted
category: testrunner
tags: guide
index: 0
title: WebdriverIO - Test Runner
---

入門
===============

WebdriverIOには、独自のテストランナーが付属しており、できるだけ早く統合テストを開始することができます。テストフレームワークを使って WebdriverIO を接続することはすべて過去のものです。WebdriverIO ランナーはあなたのためにすべての作業を行い、可能な限り効率的にテストを実行するのに役立ちます。

コマンドラインインタフェースのヘルプを表示するには、ターミナルで次のコマンドを入力してください。

```txt
$ ./node_modules/.bin/wdio --help

WebdriverIO CLI runner

Usage: wdio [options] [configFile]
config file defaults to wdio.conf.js
The [options] object will override values from the config file.

Options:
  --help, -h            prints WebdriverIO help menu
  --version, -v         prints WebdriverIO version
  --host                Selenium server host address
  --port                Selenium server port
  --path                Selenium server path (default: /wd/hub)
  --user, -u            username if using a cloud service as Selenium backend
  --key, -k             corresponding access key to the user
  --watch               watch specs for changes
  --logLevel, -l        level of logging verbosity (default: silent)
  --coloredLogs, -c     if true enables colors for log output (default: true)
  --bail                stop test runner after specific amount of tests have failed (default: 0 - don't bail)
  --screenshotPath, -s  saves a screenshot to a given path if a command fails
  --baseUrl, -b         shorten url command calls by setting a base url
  --waitforTimeout, -w  timeout for all waitForXXX commands (default: 1000ms)
  --framework, -f       defines the framework (Mocha, Jasmine or Cucumber) to run the specs (default: mocha)
  --reporters, -r       reporters to print out the results on stdout
  --suite               overwrites the specs attribute and runs the defined suite
  --spec                run only a certain spec file
  --cucumberOpts.*      Cucumber options, see the full list options at https://github.com/webdriverio/wdio-cucumber-framework#cucumberopts-options
  --jasmineOpts.*       Jasmine options, see the full list options at https://github.com/webdriverio/wdio-jasmine-framework#jasminenodeopts-options
  --mochaOpts.*         Mocha options, see the full list options at http://mochajs.org
```

いいですね！これで、テスト、機能、設定に関するすべての情報が設定された設定ファイルを定義する必要があります。そのファイルがどのように見えるかを調べるには、[設定ファイル](/guide/testrunner/configurationfile.html) セクションに切り替えます。`wdio` 設定ヘルパーを使うと、設定ファイルを簡単に生成できます。たったこれだけです。

```sh
$ ./node_modules/.bin/wdio config
```

すると、ヘルパユーティリティが起動します。あなたの答えに応じて質問します。 この方法で、1分以内に設定ファイルを生成することができます。


<div class="cliwindow" style="width: 92%">
![WDIO configuration utility](/images/config-utility.gif "WDIO configuration utility")
</div>

設定ファイルを設定したら、次のようにして統合テストを開始することができます。

```sh
$ ./node_modules/.bin/wdio wdio.conf.js
```

これだけです！これでグローバル変数の `browser` を使って selenium のインスタンスにアクセスできます。

## テストランナーをプログラムから実行する

wdio コマンドを呼び出す代わりに、テストランナーをモジュールとして含めて、任意の環境内で実行することもできます。そのためには、ランチャーモジュール（`/node_modules/webdriverio/build/launcher`）を次のように require する必要があります。

```js
var Launcher = require('webdriverio').Launcher;
```

この後、ランチャーのインスタンスを作成し、テストを実行します。 Launcher クラスは、configファイルにurlをパラメータとして渡し、configの値を上書きする[特定の](https://github.com/webdriverio/webdriverio/blob/973f23d8949dae8168e96b1b709e5b19241a373b/lib/cli.js#L51-L55)パラメータを受け取ります。

```js
var wdio = new Launcher(opts.configFile, opts);
wdio.run().then(function (code) {
    process.exit(code);
}, function (error) {
    console.error('Launcher failed to start the test', error.stacktrace);
    process.exit(1);
});
```

この run コマンドは、[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)を返します。ランチャーがテストの実行を開始できなかった場合はリジェクトされます。
