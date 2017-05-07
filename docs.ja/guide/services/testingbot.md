name: TestingBot
category: services
tags: guide
index: 2
title: WebdriverIO - TestingBot Service
---

TestingBot サービス
===================

WebdriverIO サービス。ジョブのメタデータ ('name', 'passed', 'tags', 'public', 'build', 'extra') を更新し、必要に応じて TestingBot Tunnnel を実行します。

## インストール

このサービスを使用するには、NPM からダウンロードする必要があります。

```sh
$ npm install wdio-testingbot-service --save-dev
```

## 設定

サービスを使用するには、 `wdio.conf.js` ファイルで user と key を設定し、`host` オプションを `hub.testingbot.com` に設定する必要があります。[TestingBot Tunnel](https://testingbot.com/support/other/tunnel) を使用する場合は、`tbTunnel: true` を設定するだけです。

```js
// wdio.conf.js
export.config = {
    // ...
    services: ['testingbot'],
    user: process.env.TB_KEY,
    key: process.env.TB_SECRET,
    tbTunnel: true,
    // ...
};
```

## オプション

### user
TestingBot の API KEY。

Type: `String`

### key
TestingBot の API SECRET。

Type: `String`

### tbTunnel
trueの場合、TestingBot トンネルが実行され、ブラウザテストを実行している TestingBot 仮想マシン間の安全な接続が確立されます。

Type: `Boolean`<br>
Default: `false`

### tbTunnelOpts
TestingBot Tunnel オプションを適用します (たとえば、ポート番号やログファイル設定を変更する場合) 。詳細については、[このリスト](https://github.com/testingbot/testingbot-tunnel-launcher)を参照してください。

Type: `Object`<br>
Default: `{}`
