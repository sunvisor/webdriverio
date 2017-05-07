name: autocompletion
category: usage
tags: guide
index: 8
title: WebdriverIO - Autocompletion
---

Autocompletion
=====================

しばらくプログラムコードを書いているのなら、おそらくオートコンプリートがお好きでしょう。オートコンプリートは、多くのコードエディタですぐに利用できます。 しかし、通常の場所にインストールされていないパッケージや、何らかの理由でインデックスから除外されているパッケージに対して自動補完が必要な場合は、設定変更によって追加することもできます。

![Autocompletion](http://webdriver.io/images/autocompletion/0.png)

[JSDoc](http://usejsdoc.org/) はコードの文書化に使用されます。 パラメータとその型に関するさらに詳しい情報を見るのに役立ちます。

![Autocompletion](http://webdriver.io/images/autocompletion/1.png)

使用可能なドキュメントを表示するには、IntelliJ Platform の標準ショートカット *⇧ + ⌥ + SPACE* を使用します。

![Autocompletion](http://webdriver.io/images/autocompletion/2.png)

そこで、WebStormのようなIntelliJプラットフォームのコードエディタにオートコンプリートを追加する例を考えてみましょう。

### External library としてのNode.js Core モジュール

*Settings -> Preferences -> Languages & Frameworks -> JavaScript -> Libraries* を開きます。

![Autocompletion](http://webdriver.io/images/autocompletion/3.png)

新しいライブラリを追加

![Autocompletion](http://webdriver.io/images/autocompletion/4.png)

WebdriverIO コマンドでディレクトリを追加する

![Autocompletion](http://webdriver.io/images/autocompletion/5.png)
![Autocompletion](http://webdriver.io/images/autocompletion/6.png)
![Autocompletion](http://webdriver.io/images/autocompletion/7.png)

ドキュメントのURLを入力

![Autocompletion](http://webdriver.io/images/autocompletion/8.png)
![Autocompletion](http://webdriver.io/images/autocompletion/9.png)
![Autocompletion](http://webdriver.io/images/autocompletion/10.png)

### TypeScript コミュニティ スタブを使う (TypeScript 定義ファイル)

WebStorm にはコーディングを支援するもう一つのやり方があります。それは [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) スタブをダウンロードすることです。

![Autocompletion](http://webdriver.io/images/autocompletion/11.png)
![Autocompletion](http://webdriver.io/images/autocompletion/12.png)
