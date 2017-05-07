name: allure
category: reporters
tags: guide
index: 3
title: WebdriverIO - Allure Reporter
---

Allure レポーター
=================

Allure Reporter は、テスト結果をデバッグしてエラースクリーンショットを表示するために必要なすべての情報を含むHTML生成Webサイトである [Allure](http://allure.qatools.ru/) テスト レポートを作成します。それを使用するには NPM からインストールするだけです。

```js
$ npm install wdio-allure-reporter --save-dev
```

Then add `allure` to the `reporters` array in your wdio.conf.js and define the output directory of the allure reports:
次に、 `reporters` 配列に `allure` を追加し、allure レポートの出力ディレクトリを定義します。

```js
// wdio.conf.js
exports.config = {
    // ...
    reporters: ['dot', 'allure'],
    reporterOptions: {
        allure: {
            outputDir: 'allure-results'
        }
    },
    // ...
}
```

`outputDir` デフォルトは `./allure-results` です。テストの実行が完了すると、このディレクトリには各スペックの .xml ファイルと多数の .txt や .png ファイルおよびその他の添付ファイルが格納されています。

## レポートの表示

The results can be consumed by any of the [reporting tools](http://wiki.qatools.ru/display/AL/Reporting) offered by Allure. For example:

リザルトは、Allure が提供する[レポート ツール](http://wiki.qatools.ru/display/AL/Reporting)のいずれかによって利用できます。例えば：

### Jenkins

[Allure Jenkins プラグイン](http://wiki.qatools.ru/display/AL/Allure+Jenkins+Plugin)をインストールし、正しいディレクトリから読み込むように設定します。

![Configure Allure Reporter with Jenkins](https://github.com/webdriverio/wdio-allure-reporter/raw/master/docs/images/jenkins-config.png "Configure Allure Reporter with Jenkins")

Jenkins はビルドステータスページからリザルトへのリンクを提供します：

![Allure Link](https://github.com/webdriverio/wdio-allure-reporter/raw/master/docs/images/jenkins-results.png "Allure Link")

初めてレポートを開くと、セキュリティ制限のために Jenkins がアセットを提供しないことに気付くでしょう。その場合は、Jenkinsのスクリプトコンソール（`http://<your_jenkins_instance>/script`）にアクセスして、次のセキュリティ設定を入力します。

```
System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline';")
System.setProperty("jenkins.model.DirectoryBrowserSupport.CSP", "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline';")
```

適用して Jenkins サーバーを再起動します。すべてのアセットが正しく配信されるようになりました。

### コマンドライン

[Allure コマンドライン ツール](https://www.npmjs.com/package/allure-commandline)をインストールし、results ディレクトリを処理します。

```sh
$ allure generate [allure_output_dir] && allure report open
```

This will generate a report (by default in `./allure-report`), and open it in your browser:
これにより、レポートが（デフォルトでは `./allure-report`）生成され、ブラウザで開けます

![Allure Website](https://github.com/webdriverio/wdio-allure-reporter/raw/master/docs/images/browser.png "Allure Website")
