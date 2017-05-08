name: Static Server
category: services
tags: guide
index: 7
title: WebdriverIO - Static Server Service
---

# 静的サーバー サービス

フロントエンドのアセットのみで、静的なサーバー以上の環境では実行されないプロジェクトもあります。[このサービス](https://github.com/LeadPages/wdio-static-server-service)は、テスト中に静的ファイルサーバーを実行するのに役立ちます。

## Installation

最も簡単な方法は、`package.json` の devDependencies に `wdio-static-server-service` を追加することです。

```json
{
  "devDependencies": {
    "wdio-static-server-service": "^1.0.0"
  }
}
```

簡単に次のようにもできます。

```bash
npm install wdio-static-server-service --save-dev
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください。

## 設定

静的サーバー サービスを使うには、services 配列に `static-server` を追加します。

```js
// wdio.conf.js
export.config = {
  // ...
  services: ['static-server'],
  // ...
};
```

## オプション

### staticServerFolders (必須)

フォルダーのパスとマウント ポイントの配列

Type: `Array<Object>`
Props:
 - mount `{String}` - フォルダーがマウントされる URL エンドポイント
 - path `{String}` - マウントするフォルダのパス

``` javascript
 // wdio.conf.js
 export.config = {
   // ...
   staticServerFolders: [
     { mount: '/fixtures', path: './tests/fixtures' },
     { mount: '/dist', path: './dist' },
   ],
   // ...
 };
```

### staticServerPort

サーバーをバインドするポート。

Type: `Number`

Default: `4567`

### staticServerLog

デバッグログには、マウント ポイントとリクエストを出力します。`staticServerLogs` を `true` に設定すると、コンソールに出力されます。文字列を指定するとログフォルダとして扱われます。

Type: `Boolean` or `String`
