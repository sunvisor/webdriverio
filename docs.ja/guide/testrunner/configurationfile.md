name: configurationfile
category: testrunner
tags: guide
index: 1
title: WebdriverIO - Test Runner Configuration File
---

設定ファイル
==================

設定ファイルには、テストスイートを実行するために必要なすべての情報が含まれています。 これは、JSONをエクスポートするノードモジュールです。 サポートされているすべてのプロパティと追加情報の設定例を次に示します。

```js
exports.config = {

    // =====================
    // サーバー構成
    // =====================
    // 実行中のSeleniumサーバーのホストアドレス。この情報は通常、WebdriverIOが自動的に
    // localhostに接続するときに使われません。また、Sauce Labs、Browserstack、Testing Bot
    // などのサポートされるクラウドサービスの1つを使用している場合は、WebdriverIO が
    // ユーザーとキー情報に従ってその情報を把握できるため、ホストとポートの情報を定義する
    // 必要もありません。ただし、プライベートSeleniumバックエンドを使用している場合は、
    // ここでホストアドレス、ポート、およびパスを定義する必要があります。
    //
    host: '0.0.0.0',
    port: 4444,
    path: '/wd/hub',
    //
    // ====================
    // サービスプロバイダ
    // ====================
    // プロバイダも同様に動作するはずです）。これらのサービスには、特定のユーザーとキー
    // （またはアクセスキー）の値があります。これらのサービスに接続するために、
    // ここに入力する必要があります。
    //
    user: 'webdriverio',
    key:  'xxxxxxxxxxxxxxxx-xxxxxx-xxxxx-xxxxxxxxx',
    //
    // ==================
    // テストファイル指定
    // ==================
    // 実行するテストスペックを定義します。このパターンは、`wdio` が呼び出された
    // ディレクトリからの相対パスです。NPMスクリプト
    // (https://docs.npmjs.com/cli/run-scriptを参照) から `wdio` を呼び出す場合、
    // 現在の作業ディレクトリは package.json が存在する場所であるため、
    // ` wdio`は そこから呼び出されます。
    //
    specs: [
        'test/spec/**'
    ],
    // Patterns to exclude.
    exclude: [
        'test/spec/multibrowser/**',
        'test/spec/mobile/**'
    ],
    //
    // ============
    // 機能
    // ============
    // 機能をここで定義します。 WebdriverIO は同時に複数の機能を実行できます。
    // 機能の数に応じて、WebdriverIOはいくつかのテストセッションを開始します。
    // 能力の中で、特定の機能を特定の仕様にグループ化するために、spec オプションと
    // excludeオプションを上書きすることができます。
    //
    //
    // 最初に、同時に起動するインスタンスの数を定義できます。 
    // 3つの異なる機能 (Chrome、Firefox、Safari) があり、maxInstances を1に設定したとすると、
    // wdio は3つのプロセスを生成します。そのため、10個のspec ファイルがあり、maxInstancesを
    // 10に設定すると、すべてのspecファイルが同時にテストされ、30個のプロセスが生成されます。
    // このプロパティは、基本的に、同じテストからいくつのテストを実行するかを制御します。
    //
    //
    maxInstances: 10,
    //
    // 重要な機能をすべて一緒に使うことができない場合は、Sauce Labsプラットフォーム
    // コンフィグレータをチェックしてください。これは、機能を設定するための優れたツールです。
    // https://docs.saucelabs.com/reference/platforms-configurator
    //
    capabilities: [{
        browserName: 'chrome'
    }, {
        // maxInstances can get overwritten per capability. So if you have an in house Selenium
        // grid with only 5 firefox instance available you can make sure that not more than
        // 5 instance gets started at a time.
        maxInstances: 5,
        browserName: 'firefox',
        specs: [
            'test/ffOnly/*'
        ]
    },{
        browserName: 'phantomjs',
        exclude: [
            'test/spec/alert.js'
        ]
    }],
    //
    // 有効にすると、ノードインスペクタのデバッグポートを開き、 `debugger` 
    // ステートメントで実行を一時停止します。ノードインスペクタには、次のように
    // 指定できます： `node-inspector --debug-port 5859 --no-preload`
    // デバッグ時には、テストランナーのタイムアウト間隔 (例えば、
    // jasmineNodeOpts.defaultTimeoutInterval) を非常に高い値に変更し、
    // maxInstances を1に設定することもお勧めします。
    debug: false,
    //
    // 子プロセスの開始時に使用する追加の node 引数のリスト
    execArgv: null,
    //
    //
    // ===================
    // テスト設定
    // ===================
    // ここでWebdriverIOインスタンスに関連するすべてのオプションを定義します
    //
    // デフォルトのWebdriverIOコマンドは、wdio-sync パッケージを使用して同期的に実行
    // されます。Promise を使用して非同期でテストを実行したい場合は、syncコマンドを
    // falseに設定することができます。
    sync: true,
    //
    // ログレベル: silent | verbose | command | data | result | error
    logLevel: 'silent',
    //
    // ログのカラー出力
    coloredLogs: true,
    //
    // 特定の量のテストが失敗するまでテストを実行したい時に使用します。
    // bail (default is 0 - bail しません。全てのテストを実行します).
    bail: 0,
    //
    // コマンドが失敗した時に指定したパスにスクリーンショットを保存します
    screenshotPath: 'shots',
    //
    // Set a base URL in order to shorten url command calls. If your url parameter starts
    // 短縮 URL でコマンドを実行する際のベース URL を指定します。
    // パラメータが "/" で始まるときに、baseUrl がつけられます。
    baseUrl: 'http://localhost:8080',
    //
    // waitForXXX コマンドのデフォルトのタイムアウト
    waitforTimeout: 1000,
    //
    // WebdriverIOプラグインを使用してブラウザインスタンスを初期化します。プラグイン名を
    // キーにして、プラグインのオプションをプロパティとしたオブジェクトが必要です。
    // テストを実行する前に、プラグインがインストールされていることを確認してください。
    // 現在利用可能なプラグインは次のとおりです：
    // WebdriverCSS: https://github.com/webdriverio/webdrivercss
    // WebdriverRTC: https://github.com/webdriverio/webdriverrtc
    // Browserevent: https://github.com/webdriverio/browserevent
    plugins: {
        webdrivercss: {
            screenshotRoot: 'my-shots',
            failedComparisonsRoot: 'diffs',
            misMatchTolerance: 0.05,
            screenWidth: [320,480,640,1024]
        },
        webdriverrtc: {},
        browserevent: {}
    },
    //
    // スペックを実行したいフレームワークです。
    // 次のものがサポートされています: mocha, jasmine, cucumber
    // http://webdriver.io/guide/testrunner/frameworks.htmlも参照してください。
    //
    // テストを実行する前に、特定のフレームワーク用のwdioアダプタパッケージが
    // インストールされていることを確認してください。
    framework: 'mocha',
    //
    // 標準出力へのテストレポーター
    // デフォルトでサポートしているのは 'dot' のみです。
    // http://webdriver.io/guide.html を参照して左カラムの "Reporters" をクリック
    // してください
    reporters: ['dot', 'allure'],
    //
    // 追加情報が必要なレポーターがありますのでここで定義します
    reporterOptions: {
        //
        // "xutni" レポーターを使う場合は WebdriverIO がユニットレポートを保存する
        // ディレクトリを定義します
        outputDir: './'
    },
    //
    // Mocha に渡すオプション
    // 詳しくは http://mochajs.org/ を参照
    mochaOpts: {
        ui: 'bdd'
    },
    //
    // Jasmine に渡すオプション
    // https://github.com/webdriverio/wdio-jasmine-framework#jasminenodeopts-options
    // もご覧ください
    jasmineNodeOpts: {
        //
        // Jasmine のデフォルト タイムアウト
        defaultTimeoutInterval: 5000,
        //
        // Jasmineフレームワークでは、結果に応じてアプリケーションまたはWebサイトの状態を
        // 記録するために、各アサーションをインターセプトできます。 例えば、アサーションが
        // 失敗するたびにスクリーンショットを撮るのはとても便利です。
        expectationResultHandler: function(passed, assertion) {
            // do something
        },
        //
        // ジャスミン用のgrep機能を利用する
        grep: null,
        invertGrep: null
    },
    //
    // Cucumber を使用している場合は、ステップ定義がどこにあるかを指定する必要があります。
    // https://github.com/webdriverio/wdio-cucumber-framework#cucumberopts-optionsも参照
    // してください。
    cucumberOpts: {
        require: [],        // <string[]> (file/dir) require files before executing features
        backtrace: false,   // <boolean> show full backtrace for errors
        compiler: [],       // <string[]> ("extension:module") require files with the given EXTENSION after requiring MODULE (repeatable)
        dryRun: false,      // <boolean> invoke formatters without executing steps
        failFast: false,    // <boolean> abort the run on first failure
        format: ['pretty'], // <string[]> (type[:path]) specify the output format, optionally supply PATH to redirect formatter output (repeatable)
        colors: true,       // <boolean> disable colors in formatter output
        snippets: true,     // <boolean> hide step definition snippets for pending steps
        source: true,       // <boolean> hide source URIs
        profile: [],        // <string[]> (name) specify the profile to use
        strict: false,      // <boolean> fail if there are any undefined or pending steps
        tags: [],           // <string[]> (expression) only execute the features or scenarios with tags matching the expression
        timeout: 20000,      // <number> timeout for step definitions
        ignoreUndefinedDefinitions: false, // <boolean> Enable this config to treat undefined definitions as warnings.
    },
    //
    // =====
    // フック
    // =====
    // WebdriverIOは、テストプロセスを強化し、その周りのサービスを構築するために
    // テストプロセスを横取りできる、フックを提供します。1つの関数、または一連の
    // メソッドを適用することができます。そのうちの1つが Promise を返すと、
    // WebdriverIOはその Promise が解決するまで待ちます。
    //

    /**
     * すべてのワーカーが起動する前に一度だけ実行されます。
     * @param {Object} config wdio のコンフィグ オブジェクト
     * @param {Array.<Object>} capabilities 機能詳細のリスト
     */
    onPrepare: function (config, capabilities) {
    },
    /**
     * webdriverセッションとテストフレームワークを初期化する直前に実行されます。
     * 機能やスペックに応じて設定を操作できます。
     * @param {Object} config wdio のコンフィグ オブジェクト
     * @param {Array.<Object>} capabilities 機能詳細のリスト
     * @param {Array.<String>} specs 実行されるspecファイルのパスのリスト
     */
    beforeSession: function (config, capabilities, specs) {
    },
    /**
     * テストの実行が開始される前に実行されます。 この時点で、 `browser` のようなすべての
     * グローバル変数にアクセスできます。カスタムコマンドを定義するのに最適な場所です。
     * @param {Array.<Object>} capabilities 機能詳細のリスト
     * @param {Array.<String>} specs 実行されるspecファイルのパスのリスト
     */
    before: function (capabilities, specs) {
    },
    /**
     * スイートが始まる前に実行されるフック
     * @param {Object} suite スイートの詳細
     */
    beforeSuite: function (suite) {
    },
    /**
     * スイート内でフックが開始される_前に_実行するフック（たとえば、Mocha での beforeEach 
     * 呼び出しの前に実行されます
     */
    beforeHook: function () {
    },
    /**
     * スイート内でフックが開始される_後に_実行するフック（たとえば、Mocha での beforeEach 
     * 呼び出しの後に実行されます
     */
    afterHook: function () {
    },
    /**
     * テスト (Mocha/Jasmineの場合) またはステップ (Cucumberの場合) が開始する前に実行される関数
     * @param {Object} test テストの詳細
     */
    beforeTest: function (test) {
    },
    /**
     * WebdriverIO コマンドが実行される前に実行されます。
     * @param {String} commandName フック コマンド名
     * @param {Array} args コマンドが受け取る引数
     */
    beforeCommand: function (commandName, args) {
    },
    /**
     * WebdriverIO コマンドが実行した後で実行されます。
     * @param {String} commandName フック コマンド名
     * @param {Array} args コマンドが受け取った引数
     * @param {Number} result 0 - 成功, 1 - コマンドエラー
     * @param {Object} error エラーオブジェクト
     */
    afterCommand: function (commandName, args, result, error) {
    },
    /**
     * テスト (Mocha/Jasmineの場合) またはステップ (Cucumberの場合) の後に実行される関数
     * @param {Object} test テストの詳細
     */
    afterTest: function (test) {
    },
    /**
     * スイートが終わった後に実行されるフック
     * @param {Object} suite スイートの詳細
     */
    afterSuite: function (suite) {
    },
    /**
     * 全てのテストが完了した時に実行されます。テストからのグローバル変数にアクセス可能です
     * @param {Number} result 0 - テストはパスした, 1 - テストは失敗した
     * @param {Array.<Object>} capabilities 機能詳細のリスト
     * @param {Array.<String>} specs 実行されるspecファイルのパスのリスト
     */
    after: function (result, capabilities, specs) {
    },
    /**
     * webdriver セッションが終了した後に実行されます。
     * @param {Object} config wdio のコンフィグ オブジェクト
     * @param {Array.<Object>} capabilities 機能詳細のリスト
     * @param {Array.<String>} specs 実行されるspecファイルのパスのリスト
     */
    afterSession: function (config, capabilities, specs) {
    },
    /**
     * Gets executed after all workers got shut down and the process is about to exit.
     * すべてのワーカーがシャットダウンしプロセスが終了する時に実行されます。

     * @param {Object} exitCode 0 - 成功, 1 - 失敗
     * @param {Object} config wdio のコンフィグ オブジェクト
     * @param {Array.<Object>} capabilities 機能詳細のリスト
     */
    onComplete: function (exitCode, config, capabilities) {
    },
    //
    // Cucumber specific hooks
    beforeFeature: function (feature) {
    },
    beforeScenario: function (scenario) {
    },
    beforeStep: function (step) {
    },
    afterStep: function (stepResult) {
    },
    afterScenario: function (scenario) {
    },
    afterFeature: function (feature) {
    }
};
```

このファイルは、[サンプル フォルダ](https://github.com/webdriverio/webdriverio/blob/master/examples/wdio.conf.js) のすべての可能なオプションとバリエーションで見つけることもできます。
