name: organizing suites
category: testrunner
tags: guide
index: 4
title: WebdriverIO - Organize Test Suite
---

テスト スイートの構成
===================

プロジェクトが成長してくると、必然的に次々に統合テストが追加されます。これによりビルド時間が長くなり、生産性も低下します。これを防ぐには、テストを並行して実行する必要があります。WebdriverIOがそれぞれのスペック（または Cucumber のフィーチャ ファイル）に対して1つのSeleniumセッションを作成することは、既にご存じかもしれません。一般に、アプリケーション内の1つの機能を1つのスペックファイルでテストするようにしてください。 1つのファイルに入れるテストは多すぎたり、少なすぎたりしないようにしてください。ただし、それについての黄金のルールはありません。

どんどんスペックファイルができたら、それらを同時に実行する必要があります。これを行うには、設定ファイルの[`maxInstances`](https://github.com/webdriverio/webdriverio/blob/master/examples/wdio.conf.js#L52-L60)プロパティを調整します。

WebdriverIO では、テストを最大限の同時実行でテストできます。つまり、ファイルやテストの数に関わらず、すべてを平行して実行できます。
ただし、特定の制限（コンピュータCPU、同時実行制限）があります。

> 3つの異なる capabilities（Chrome、Firefox、Safari）があり、maxInstances を1に設定したとすると、wdioテストランナーは3つのプロセスを生成します。したがって、10個のspecファイルがあり、maxInstancesを10に設定した場合、すべてのスペックファイルが同時にテストされ、30のプロセスが生成されます。

`maxInstance` プロパティをグローバルに定義して、すべてのブラウザの属性として設定できます。 独自の Selenium グリッドを実行している場合は、他のブラウザーよりも1つのブラウザーに多くの容量がある可能性があります。この場合、capability オブジェクトの `maxInstance` を制限することができます。

```js
// wdio.conf.js
exports.config = {
    // ...
    // 全てのブラウザの maxInstances をセット
    maxInstances: 10,
    // ...
    capabilities: [{
        browserName: "firefox"
    }, {
        // maxInstances は capacity ごとに上書きされます。
        // したがって、5つのfirefoxインスタンスしか使用できない内部のSeleniumグリッド
        // を使用している場合に、一度に5つ以上のインスタンスが起動しないようにすること
        // ができます。
        browserName: 'chrome'
    }],
    // ...
}
```

## メインの設定ファイルからの継承



複数の環境（例えば、開発とか統合）でテストスイートを実行する場合、管理しやすくするために、複数の設定ファイルを用意すると便利です。
[ページオブジェクトの概念](/guide/testrunner/pageobjects.html)と同様に、最初にメインの設定ファイルを作成します。
そこには、環境全体で共有するすべての構成が含まれています。
次に、環境ごとにファイルを作成し、メインの設定ファイルから環境固有の情報を補完することができます。

```js
// wdio.dev.config.js
var merge = require('deepmerge');
var wdioConf = require('./wdio.conf.js');

// have main config file as default but overwrite environment specific information
// デフォルトの main 設定ファイルがありますが、環境固有の情報を上書きします
exports.config = merge(wdioConf.config, {
    capabilities: [
        // 追加の capability をここに
        // ...
    ],

    // ローカルではなく sauce でテストを実行する
    user: process.env.SAUCE_USERNAME,
    key: process.env.SAUCE_ACCESS_KEY,
    services: ['sauce']
});

// レポーターを追加する
exports.config.reporters.push('allure');
```

## グループ テスト スペック

簡単にスイート内のテスト スペックをグループ化し、それらのすべてではなく1つのスイートを実行できます。これを行うには、最初にあなたの wdio 設定でスイートを定義する必要があります。

```js
// wdio.conf.js
exports.config = {
    // 全てのテストを定義する
    specs: ['./test/specs/**/*.spec.js'],
    // ...
    // 特定のスイートを定義する
    suites: {
        login: [
            './test/specs/login.success.spec.js',
            './test/specs/login.failure.spec.js'
        ],
        otherFeature: [
            // ...
        ]
    },
    // ...
}
```

ここで1つのスイートを実行したい場合は、次のようにスイート名をコマンドラインの引数に渡します。

```sh
$ wdio wdio.conf.js --suite login
```

複数のスイートを一度に実行する場合は

```sh
$ wdio wdio.conf.js --suite login,otherFeature
```

## 1つのテスト スイートを実行する

WebdriverIO テストで作業している場合は、アサーションなどのコードを追加するたびにスイート全体を実行する必要はありません。`--spec` パラメータを使用すると、実行する suite (Mocha、Jasmine) または feature (Cucumber) を指定できます。 たとえば、ログインテストのみを実行する場合は、次のようにします。

```sh
$ wdio wdio.conf.js --spec ./test/specs/e2e/login.js
```

各テストファイルは、1つのテストランナー プロセスで実行されることに注意してください。あらかじめファイルをスキャンしていないので、例えば、スペックファイルの先頭 `describe.only` を置いて、Mocha にそのスイートのみを実行するように指定することは_できません_。この機能は、これを同じ方法で行うのに役立ちます。

## 障害発生時にテストを中止する

`bail` オプションを使うと、WebdriverIO がテスト失敗後にテスト実行を停止するタイミングを指定できます。これは大きなテストスイートがあって、既にビルドが壊れていとわかっているときに、長いテストを実行させたくない場合に役立ちます。このオプションには、スペックの失敗が何度起きたらテスト実行全体を停止させるべきかを指定する数値を設定します。デフォルトは0で、常にすべてのテスト スペックが実行されます。
