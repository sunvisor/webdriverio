name: Webpack
category: services
tags: guide
index: 8
title: WebdriverIO - Webpack Service
---

WDIO Webpack サービス
=====================

これにより、テストの前に webpack によって静的アセットをビルドすることができます。

## インストール

最も簡単な方法は、`package.json` の devDependencies に `wdio-webpack-service` を追加することです。

```json
{
  "devDependencies": {
    "wdio-webpack-service": "^1.0.0"
  }
}
```

簡単に次のようにもできます。

```bash
npm install wdio-webpack-service --save-dev
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください。

## 設定

Webpack サービスを使うには、services 配列に `webpack` を追加します。

```js
// wdio.conf.js
export.config = {
  // ...
  services: ['webpack'],
  // ...
};
```

## オプション

### webpackConfig (必須)

バンドルを構築するために使用する webpack の設定。詳細については、[webpackのドキュメント](http://webpack.github.io/docs/configuration.html)を参照してください。

Type: `Object`

### webpackDriverConfig

このオプションを使用すると、ドライバの webpack config の設定の一部を無効にすることができます。これを使用して、ファイルの配置場所などを変更することができます。設定は、[`webpack-merge`](https://github.com/survivejs/webpack-merge) プロジェクトとスマート ストラテジーを使用してマージされます。

Type: `Object`

### webpackLog

デバッグログは、マウントポイントとリクエストを出力します。 `webpackLogs` が `true` に設定されると、コンソールに印刷されます。文字列を指定するとログフォルダとして扱われます。

Type: `Boolean` or `String`
