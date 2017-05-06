name: jenkins
category: testrunner
tags: guide
index: 7
title: WebdriverIO - Test Runner Jenkins Integration
---

Jenkins との統合
===================

WebdriverIOは、 [Jenkins](https://jenkins-ci.org/) のようなCIシステムとの緊密な統合を提供します。[junit レポーター](https://github.com/webdriverio/wdio-junit-reporter)を使用すると、テストを簡単にデバッグするだけでなく、テスト結果を追跡することができます。統合するのはとても簡単です。このチュートリアルで使用した[デモプロジェクト](https://github.com/christian-bromann/wdio-demo)では、WebdriverIO テスト スイートをJenkinsと統合する方法を実演しています。

まず、 `junit` をテスト レポーター報告者として定義する必要があります。また、それがインストール (`$ npm install --save-dev wdio-junit-reporter`) されていることを確認してください、そうすると xenit の結果を Jenkins が取得できる場所に保存します。ですから、次のように config 内にレポーターを定義します。

```js
// wdio.conf.js
module.exports = {
    // ...
    reporters: ['dot', 'junit'],
    reporterOptions: {
        junit: {
            outputDir: './'
        }
    },
    // ...
};
```

フレームワークはお好きなものを選択してください。レポーターも同様です。このチュートリアルでは、Jasmine を使用します。 [いくつかのテスト](https://github.com/christian-bromann/wdio-demo/tree/master/test/specs)を書いたら、新しい Jenkins ジョブをセットアップすることができます。名前と説明を付けてください。

![Name And Description](/images/jenkins-jobname.png "Name And Description")

次に、常にリポジトリの最新バージョンを取得するようにします。

![Jenkins Git Setup](/images/jenkins-gitsetup.png "Jenkins Git Setup")

ここで重要なのは、シェルコマンドを実行するビルド ステップを作成することです。ビルド ステップでは、プロジェクトをビルドする必要があります。このデモプロジェクトは外部アプリケーションのみをテストするのでビルドする必要はありませんが、ノードの依存関係をインストールして、`node_modules/.bin/wdio test/wdio.conf.js` のエリアスである `test` コマンドを実行します。

![Build Step](/images/jenkins-runjob.png "Build Step")

テストの後、Jenkins が xunit レポートをトラックするようにします。これを行うには、_"Publish JUnit test result report"_ というビルド後のアクションを追加する必要があります。また、外部の xunit プラグインをインストールしてレポートを追跡することもできます。JUnit には基本的な Jenkinsi のインストールが付属しているのでこれだけでOKです。

設定ファイルによると、xunit レポートは私たちのワークスペースのルートディレクトリに保存されます。これらのレポートはXMLファイルです。レポートを追跡するために必要なことは、Jenkins をルートディレクトリのすべてのxmlファイルにポイントさせることです。

![Post-build Action](/images/jenkins-postjob.png "Post-build Action")

That's it! This is all you need to setup Jenkins to run your WebdriverIO jobs. The only thing that didn't got mentioned is that Jenkins is setup in a way that it runs Node.js v0.12 and has the [Sauce Labs](https://saucelabs.com/) environment variables set in the settings.

以上です！WebdriverIO ジョブを実行するようにJenkinsをセットアップするために必要なのはこれだけです。ここで言及されていない唯一のことは、Jenkins が Node.js v0.12を実行し、設定に[Sauce Labs](https://saucelabs.com/) 環境変数を設定するように設定されていることです。


これで、履歴グラフ、失敗したジョブのスタックトレース情報、各テストで使用されたペイロードを持つコマンドのリストを含む詳細なテスト結果が提供されるようになります。

![Jenkins Final Integration](/images/jenkins-final.png "Jenkins Final Integration")
