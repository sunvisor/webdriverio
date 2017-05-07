name: Browserstack
category: services
tags: guide
index: 1
title: WebdriverIO - Browserstack Service
---

Browserstack サービス
=====================

Browserstack ユーザー用のローカル トンネルとジョブのメタデータを管理する WebdriverIO サービス。

## インストール

次を実行します。

```bash
npm install --save-dev wdio-browserstack-service
```

## 設定

WebdriverIO にはすでに Browserstack のサポートがあります。wdio.conf.js ファイルに `user` と `key` を設定するだけです。このサービスプラグインは、[Browserstack Tunnel](https://www.browserstack.com/automate/node#setting-local-tunnel) のサポートを提供します。この機能を有効にするには、 `browserstackLocal: true` も設定します。

```js
// wdio.conf.js
export.config = {
  // ...
  services: ['browserstack'],
  user: process.env.BROWSERSTACK_USERNAME,
  key: process.env.BROWSERSTACK_ACCESS_KEY,
  browserstackLocal: true,
};
```

## オプション

### user
Browserstack のユーザー名

Type: `String`

### key
Browserstack のアクセスキー

Type: `String`

### browserstackLocal
これをtrueに設定すると、Browserstack クラウドからコンピュータへのルーティング接続が有効になります。

Type: `Boolean`<br>
Default: `false`

### browserstackOpts
指定されたオプションはBrowserstackLocalに渡されます。 詳細は[このリスト](https://www.browserstack.com/local-testing#modifiers)を参照してください。

Type: `Object`<br>
Default: `{}`

## 既知の問題

WebdriverIO がマルチプロセスモデルをどのように設計したかの詳細です。localIdentifier を子プロセスに確実に転送することは不可能ではないにしても、非常に困難です。現時点では識別子なしで使用することをお勧めします。これにより、アカウント全体のローカルトンネルが作成されます。
