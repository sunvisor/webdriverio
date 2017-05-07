name: cloudservices
category: usage
tags: guide
index: 2
title: WebdriverIO - Using Cloud Services
---

# Using Cloud Services
# クラウドサービスを使う

WebdriverIOはで Sauce Labs、Browserstack、TestingBot などのオンデマンドサービスを簡単に使用できます。

1. WebdriverIOが `host` コンフィグを設定するか、`user`と `key` の値に基づいて自動的にWebdriverIOを設定するかのいずれかによって、WebdriverIOがSeleniumサーバーとしてホスト (例：Sauce Labsの場合は `ondemand.saucelabs.com` ) を使用していることを確認してください。
2. (オプション) 各ブラウザのサービス固有の値を `desiredCapabilities` に設定します (例えば、`build` はビルド番号を指定し、複数のテストをまとめてビルドします) 。
3. (オプション) テストがローカル`localhost`　にアクセスできるように、プロバイダへのローカルトラフィックをトンネルします。

Travisでクラウドサービスのみを実行する場合は、`CI` 環境変数を使用して Travis にいるかどうかを確認し、それに応じて設定を変更することができます。

```javascript
// wdio.conf.js

var config = {...}
if (process.env.CI) {
    config.user = process.env.SAUCE_USERNAME;
    config.key = process.env.SAUCE_ACCESS_KEY;
}
exports.config = config
```

## [Sauce Labs](https://saucelabs.com/)

Sauce Labs でリモートで実行するようにテストを設定するのは簡単です。

唯一の要件は、コンフィグの `user` と `key` にあなたの Souce Labs のユーザー名とアクセスキーを設定する事です。(`wdio.conf.js` でエクスポートされるか、`webdriverio.remote(...)` に渡す)

オプションの[テスト設定オプション](https://docs.saucelabs.com/reference/test-configuration/#webdriver-api)を、すべてのブラウザの機能のキー／バリュー ペアとして渡すこともできます。

### [Sauce Connect](https://wiki.saucelabs.com/display/DOCS/Sauce+Connect+Proxy)

インターネットにアクセスできないサーバー (`localhost`など) に対してテストを実行する場合は、Sauce Connect を使用する必要があります。

これをサポートするのはWebdriverIO の範囲外です。したがって、自分で起動する必要があります。

WDIO テストランナーを使用している場合は、[`wdio-sauce-service`](https://github.com/webdriverio/wdio-sauce-service) をダウンロードして `wdio.conf.js` を設定します。これは、Sauce Connectを実行するのに役立ち、Sauce サービスにあなたのテストをより良く統合するための追加機能を備えています。

### Travis CI と共に

Travis CI は、各テストの前に Sauce Connect を開始する[ことをサポート](http://docs.travis-ci.com/user/sauce-connect/#Setting-up-Sauce-Connect)していますので、興味がある場合は指示に従ってください。

そうする場合は、各ブラウザの機能で `tunnel-identifier` テスト設定オプションを設定する必要があります。 Travisはこれをデフォルトで TRAVIS_JOB_NUMBER 環境変数に設定します。

また、Sauce Labs でビルド番号でテストをグループ化する場合は、`build` を `TRAVIS_BUILD_NUMBER` に設定できます。

最後に`name` を設定すると、このビルドのSauce Labsでこのテストの名前が変更されます。 WDIOテストランナーと [`wdio-sauce-service`](https://github.com/webdriverio/wdio-sauce-service) WebdriverIOを組み合わせて使用している場合、テストの適切な名前が自動的に設定されます。

サンプル `desiredCapabilities`:

```javascript
browserName: 'chrome',
version: '27.0',
platform: 'XP',
'tunnel-identifier': process.env.TRAVIS_JOB_NUMBER,
name: 'integration',
build: process.env.TRAVIS_BUILD_NUMBER
```

### タイムアウト

テストをリモートで実行しているので、タイムアウトを増やす必要があります。

[idle timeout](https://docs.saucelabs.com/reference/test-configuration/#idle-test-timeout) をテスト構成オプションとして渡すことによって、アイドルタイムアウトを変更できます。これは、接続を閉じる前に Sauce がコマンドの間に待機する時間を制御します。

## [BrowserStack](https://www.browserstack.com/)

Browserstack も簡単にサポートされています。

The only requirement is to set the `user` and `key` in your config (either exported by `wdio.conf.js` or passed into `webdriverio.remote(...)`) to your Browserstack automate username and access key.
唯一の必要条件は、あなたの設定 ( wdio.conf.jsによってエクスポートされるか、 webdriverio.remote(...)渡されるwebdriverio.remote(...) userとkeyを設定して、Browserstackのユーザー名とアクセスキーを自動化することです。

You can also pass in any optional [supported capabilities](https://www.browserstack.com/automate/capabilities) as a key/value in the capabilities for any browser. If you set `browserstack.debug` to `true` it will record a screencast of the session, which might be helpful.
任意のサポートされている機能を、すべてのブラウザの機能のキー/値として渡すこともできます。 browserstack.debugをtrueすると、セッションのスクリーンキャストが記録されます。

### [Local Testing](https://www.browserstack.com/local-testing#command-line)
ローカルテスト

If you want to run tests against a server that is not accessible to the Internet (like on `localhost`), then you need to use Local Testing.
インターネットにアクセスできないサーバー ( localhostなど) に対してテストを実行する場合は、ローカルテストを使用する必要があります。

It is out of the scope of WebdriverIO to support this, so you must start it by yourself.
これをサポートするのはWebdriverIOの範囲外です。したがって、自分で起動する必要があります。

If you do use local, you should set `browserstack.local` to `true` in your capabilities.
localを使用するtrueは、browserstack.localをtrueに設定する必要があります。

If you are using the WDIO testrunner download and configure the [`wdio-browserstack-service`](https://github.com/itszero/wdio-browserstack-service) in your `wdio.conf.js`. It helps getting BrowserStack running and comes with additional features that better integrate your tests into the BrowserStack service.
WDIOテストランナーを使用している場合は、 wdio-browserstack-serviceをダウンロードしてwdio.conf.jsます。 これは、BrowserStackを実行するのに役立ち、BrowserStackサービスにテストをよりよく統合するための追加機能を備えています。

### With Travis CI

If you want to add Local Testing in Travis you have to start it by yourself.
Travisにローカルテストを追加する場合は、自分で起動する必要があります。

The following script will download and start it in the background. You should run this in Travis before starting the tests.
次のスクリプトは、バックグラウンドでダウンロードして開始します。 テストを開始する前に、これをTravisで実行する必要があります。

```bash
wget https://www.browserstack.com/browserstack-local/BrowserStackLocal-linux-x64.zip
unzip BrowserStackLocal-linux-x64.zip
./BrowserStackLocal -v -onlyAutomate -forcelocal $BROWSERSTACK_ACCESS_KEY &
sleep 3
```

Also, you might wanna set the `build` to the Travis build number.
また、 buildをTravisのビルド番号に設定buildこともできます。

Example `desiredCapabilities`:

```javascript
browserName: 'chrome',
project: 'myApp',
version: '44.0',
build: 'myApp #' + process.env.TRAVIS_BUILD_NUMBER + '.' + process.env.TRAVIS_JOB_NUMBER,
'browserstack.local': 'true',
'browserstack.debug': 'true'
```

## [TestingBot](https://testingbot.com/)

The only requirement is to set the `user` and `key` in your config (either exported by `wdio.conf.js` or passed into `webdriverio.remote(...)`) to your TestingBot username and secret key.
唯一の要件は、コンフィグの `user` と `key` にあなたの Souce Labs のユーザー名とアクセスキーを設定する事です。(`wdio.conf.js` でエクスポートされるか、`webdriverio.remote(...)` に渡す)

任意の[サポートされる capabilities](https://testingbot.com/support/other/test-options) を、すべてのブラウザの機能のキー／値として渡すこともできます。

### [ローカルテスト](https://testingbot.com/support/other/tunnel)

インターネットにアクセスできないサーバー (`localhost`など) に対してテストを実行する場合は、ローカルテストを使用する必要があります。TestingBot は、インターネットからアクセスできない WebサイトをテストするためのJAVAベースのトンネルを提供します。

彼らのトンネルサポートページには、これを稼働させるのに必要な情報が含まれています。

WDIOテストランナーを使用している場合は、[`wdio-testingbot-service`](https://github.com/testingbot/wdio-testingbot-service) をダウンロードして `wdio.conf.js` を設定します。TestingBot を実行するのに役立ち、TestingBot サービスにあなたのテストをより良く統合するための追加機能が付属しています。
