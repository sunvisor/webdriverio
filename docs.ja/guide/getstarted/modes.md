name: modes
category: getstarted
tags: guide
index: 3
title: WebdriverIO - How to use WebdriverIO
---

# WebdriverIO の使い方

WebdriverIOはさまざまな目的で使用できます。Webdriver プロトコルAPIを実装し、ブラウザを自動化された方法で実行できます。このフレームワークは、任意の環境やあらゆる種類の作業に対応するように設計されています。サードパーティ製のフレームワークとは独立しており、実行するのに必要なのは Node.js だけです。


### スタンドアロン モード

WebdriverIOを実行する最も簡単な形式は、スタンドアロンモードです。 これは、通常 `selenium-server-standalone` と呼ばれる Selenium サーバーファイルとは関係ありません。基本的には、プロジェクト内で webdriverio パッケージが必要で、その背後にあるAPIを使用して自動化を実行するだけです。簡単な例を次に示します。

```js
var webdriverio = require('webdriverio');
var options = { desiredCapabilities: { browserName: 'chrome' } };
var client = webdriverio.remote(options);

client
    .init()
    .url('https://duckduckgo.com/')
    .setValue('#search_form_input_homepage', 'WebdriverIO')
    .click('#search_button_homepage')
    .getTitle().then(function(title) {
        console.log('Title is: ' + title);
        // outputs: "Title is: WebdriverIO (Software) at DuckDuckGo"
    })
    .end();
```

スタンドアロン モードで WebdriverIO を使用すると、この自動化ツールを独自の（テスト）プロジェクトに統合して、新しい自動化ライブラリを作成できます。その代表例は[Chimp](https://chimp.readme.io/) or [CodeceptJS](http://codecept.io/) です。また、プレーンな Node スクリプトを書いて Web 上のコンテンツや実行中のブラウザが必要な場所をスクレイピングすることもできます。

### WDIO テストランナー


WebdriverIOの主な用途は、エンドトゥエンドの大規模なテストを行うことです。そのために、私たちは読みやすく、維持しやすい信頼性の高いテストスイートを構築するのに役立つテストランナーを実装しました。 テストランナーは、プレーンな自動化ライブラリで、作業時によく直面する多くの問題を処理します。1つは、テスト実行を管理してテスト仕様を分割し、テストを最大同時実行で実行できるようにすることです。 また、セッション管理を処理し、問題のデバッグやテストでのエラーの発見に役立つ多くの機能を提供します。上記のテストスペックと同じ例をwdioで実行してみましょう。

```js
describe('DuckDuckGo search', function() {
    it('searches for WebdriverIO', function() {
        browser.url('https://duckduckgo.com/');
        browser.setValue('#search_form_input_homepage', 'WebdriverIO');
        browser.click('#search_button_homepage');

        var title = browser.getTitle();
        console.log('Title is: ' + title);
        // outputs: "Title is: WebdriverIO (Software) at DuckDuckGo"
    });
});
```

テストランナーは、Mocha、Jasmine または Cucumber などの一般的なテストフレームワークを抽象化します。スタンドアロン モードとは異なり、wdio テストランナーによって実行されるすべてのコマンドは同期的です。これは、非同期コードを処理するために、もう Promise は使わないことを意味します。wdio テストランナーを使用してテストを実行するには、[Getting Started](/../testrunner/gettingstarted.md) のセクションを参照してください。
