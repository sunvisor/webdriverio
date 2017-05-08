name: firefox profile
category: services
tags: guide
index: 5
title: WebdriverIO - Firefox Profile Service
---

WDIO Firefox Profile サービス
=============================

特定の拡張機能を持っているとか 、ある種の設定をした Firefox ブラウザを実行したいということがありますか？
Selenium では、プロファイルを `base64` 文字列として、`firefox_profile` プロパティに渡すことで、Firefox ブラウザ用のプロファイルを使用できます。
それには、そのプロファイルをビルドし、それを `base64` に変換する必要があります。[wdio テストランナー](http://webdriver.io/guide/testrunner/gettingstarted.html)用のこのサービスでは、手でプロファイルをコンパイルする必要が無くなります。その後 `wdio.conf.js` ファイルで好みのオプションを定義しましょう。

設定可能なオプションを見つけるには、Firefoxブラウザの [about:config](about:config) を開くか、[mozillaZine](http://kb.mozillazine.org/About:config_entries) の Web サイトにアクセスして、各設定に関するドキュメントを見つけてください。それに加えて、テストが始まる前にインストールされるべきコンパイル済み (`*.xpi`) のFirefox拡張を定義することもできます。

## インストール

最も簡単な方法は、`package.json` の devDependencies に `wdio-firefox-profile-service` を追加することです。

```json
{
  "devDependencies": {
    "wdio-firefox-profile-service": "~0.1"
  }
}
```

簡単に次のようにもできます。

```bash
npm install wdio-firefox-profile-service --save-dev
```

`WebdriverIO` のインストール方法については[こちら](http://webdriver.io/guide/getstarted/install.html)をご覧ください。

## 設定

services リストに `firefox-profile` サービスを追加して、プロファイルを設定します。次に、`firefoxProfile` プロパティの設定を次のように定義します。

```js
// wdio.conf.js
export.config = {
  // ...
  services: ['firefox-profile'],
  firefoxProfile: {
      extensions: ['/path/to/extensionA.xpi', '/path/to/extensionB.xpi'],
      'browser.startup.homepage': 'http://webdriver.io'
  },
  // ...
};
```

## オプション

### firefoxProfile

キー／バリュー ペアとしてすべての設定が含まれます。拡張機能を追加する場合は、使用する拡張機能へのパス文字列の配列を持つ `extensions` キーを使用します。

Type: `Object`
