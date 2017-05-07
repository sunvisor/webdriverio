name: sauce
category: services
tags: guide
index: 0
title: WebdriverIO - Sauce Service
---

Sauce サービス
==============

このサービスは、WebdriverIO と WDIO テストランナーを [Sauce Labs](https://saucelabs.com/) サービスと統合するのに役立ちます。自動的にジョブのステータスが設定され、ジョブ名、タグ、可用性、カスタムデータなどの重要なジョブプロパティがすべて更新されます。[Sauce Connect](https://wiki.saucelabs.com/display/DOCS/Sauce+Connect+Proxy) が統合されていると、すべてのテストを安全なトンネルで実行するために、設定にわずかな微調整しか必要ありません。

上記のとおり、このサービスはあなたのテストを Sauce サービスとよりよく統合するのに役立ちます。これを示す良い例として、ジョブのアノテーションがあります。これらは、特定の時点でジョブにアノテーションを付けて、その詳細をジョブの詳細ページで簡単に見つけることができる特別なコマンドです。

![Job Annotations](http://www.christian-bromann.com/a.png "Job Annotations")

この画像は、 [この Cucumber テスト](https://github.com/webdriverio/webdriverio/blob/master/examples/wdio/runner-specs/cucumber/features/my-feature.feature)を実行しているSauce Labsのジョブの詳細ページのコマンドリストを示しています。 このサービスでは、自動的にジョブアノテーションが作成され、テスト失敗の原因となったコマンドを簡単に見つけることができます。

## インストール

サービスを使用するには、NPM からダウンロードする必要があります。

```sh
$ npm install wdio-sauce-service --save-dev
```

## Configuration

サービスを使用するには、wdio.conf.js ファイルで `user` と `key` を設定する必要があります。自動的に Sauce Labs を使用して統合テストを実行します。Sauce Connect を使用する場合は、`sauceConnect：true` を設定するだけです。

```js
// wdio.conf.js
export.config = {
    // ...
    services: ['sauce'],
    user: process.env.SAUCE_USERNAME,
    key: process.env.SAUCE_ACCESS_KEY,
    sauceConnect: true,
    // ...
};
```

## Options

### user
Sauce Labs のあなたのユーザー名。

Type: `String`

### key
Sauce Labsのあなたのアクセスキー。

Type: `String`

### sauceConnect
`true` の場合は、Sauce Connect を実行し、ブラウザテストを実行している Sauce Labs 仮想マシン間を安全に接続します。

Type: `Boolean`<br>
Default: `false`

### sauceConnectOpts
Sauce Connect オプションを適用します (たとえば、ポート番号やログファイル設定を変更する場合) 。詳細については、 [このリスト](https://github.com/bermi/sauce-connect-launcher#advanced-usage)を参照してください。

Type: `Object`<br>
Default: `{}`
