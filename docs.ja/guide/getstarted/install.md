name: install
category: getstarted
tags: guide
index: 0
title: WebdriverIO - Install
---

# インストール

You will need to have [Node.js](http://nodejs.org/) and [NPM](https://www.npmjs.org/) installed on your machine. Check out their project websites for more instructions. If you want to have WebdriverIO integrated into your test suite, then install it locally with:

[Node.js](http://nodejs.org/) と [NPM](https://www.npmjs.org/) をインストールする必要があります。詳細については、プロジェクトのウェブサイトをご覧ください。 WebdriverIO をテストスイートに統合するには、次の方法でローカルにインストールします。

```sh $ npm install webdriverio --save-dev
```

テストランナーは `wdio` と呼ばれ、インストールが成功したら利用できます。

```sh
$ ./node_modules/.bin/wdio --help
```

パッケージをグローバルにインストールして、コマンドラインから直接 `wdio` を使用することもできます。ですが、プロジェクトごとにインストールすることをお勧めします。

## Selenium 環境をセットアップする

Selenium 環境をセットアップするには、スタンドアロンパッケージとしてする方法と、サーバとブラウザドライバを別々にインストールする方法があります。

### 既存のスタンドアロンパッケージの使用

最も簡単な方法は、 [vvo/selenium-standalone](https://github.com/vvo/selenium-standalone) などの NPM selenium スタンドアロンパッケージの1つを使用することです。インストール後（グローバルに）、次のコマンドを実行するとサーバーを起動できます。

```sh
$  selenium-standalone start
```

wdio のテストランナーを使う場合は、[Selenium Standalone Service](/guide/services/selenium-standalone.html) に興味があるかもしれません。これはテスト開始前に毎回サーバーを起動します。

テストセットアップとは別に selenium サーバーを実行することもできます。そうするには、[ここ](http://docs.seleniumhq.org/download/)から selenium スタンドアロンサーバーの最新バージョンを入手する必要があります。あなたのシステムのどこかにjarファイルをダウンロードするだけです。その後、ターミナルを起動してファイルを実行してください。

```sh
$ java -jar /your/download/directory/selenium-server-standalone-3.0.1.jar
```

Selenium の最新バージョンでは、ブラウザ用のドライバのほとんどは、ダウンロードして設定する必要がある外部ドライバが付属しています。

## Chrome の設定

[Chromedriver](https://sites.google.com/a/chromium.org/chromedriver/home) は、Chromium 用の WebDriver ワイヤプロトコルを実装するスタンドアロンサーバーです。Chromium と WebDriver チームのメンバーによって開発されています。ローカルマシンでChromeブラウザテストを実行するには、プロジェクトウェブサイトから ChromeDriver をダウンロードし、 PATH を ChromeDriver 実行可能ファイルに設定して、マシン上で使用可能にする必要があります。

## Firefox の設定

Firefox でも同様です。GeckoDriverは、呼び出しを [Marionette](https://developer.mozilla.org/en-US/docs/Mozilla/QA/Marionette) の自動化プロトコルに変換します。Selenium standalone server v3 以降のFirefoxでテストを実行するには、[ここ](https://github.com/mozilla/geckodriver/releases) から最新のドライバをダウンロードし、マシンのPATHで利用可能にする必要があります。
