layout: start
---

<aside class="teaser">
    <div class="teaserbox">
        <h3>Extendable</h3>
        <p>
            Adding helper functions, or more complicated sets and combi-<br>nations of existing
            commands is **simple** and really **useful**
        </p>
    </div>
    <div class="teaserbox">
        <h3>Compatible</h3>
        <p>
            WebdriverIO works in combination with most of the **TDD** and **BDD** test frameworks
            in the JavaScript world
        </p>
    </div>
    <div class="teaserbox">
        <h3>Feature-Rich</h3>
        <p>
            It implements all Webdriver protocol commands and provides **useful integrations** with other tools.
        </p>
    </div>
</aside>

<aside class="features">
    <ul>
        <li>synchronous command handling</li>
        <li>supports a variety of hooks</li>
        <li>command line interface support</li>
    </ul>
    <ul>
        <li><a href="https://twitter.com/webdriverio/status/806911722682544128" target="_blank">REPL interface</a></li>
        <li><a href="http://gulpjs.com/">Gulp</a> and <a href="http://gruntjs.com/">Grunt</a> support</li>
        <li>selector chaining</li>
    </ul>
</aside>

<div class="rulethemall">
    <h2 class="text-align">One Tool To Rule Them All:</h2>
    <a href="https://github.com/webdriverio/grunt-webdriver"><img src="/images/plugins/grunt.png" alt="Grunt Integration"></a>
    <a href="https://github.com/webdriverio/gulp-webdriver"><img src="/images/plugins/gulp.png" alt="Gulp Integration"></a>
    <a href="https://atom.io/packages/webdriverio-snippets"><img src="http://webdriver.io/images/plugins/atom.png" alt="Atom snippets for WebdriverIO API"></a>
    <a href="https://packagecontrol.io/packages/WebdriverIO"><img src="/images/plugins/sublime.png" alt="Sublime Text Plugin"></a>
    <a href="https://github.com/webdriverio/webdrivercss#applitools-eyes-support"><img src="/images/plugins/applitools.png" alt="Visual Regression Testing with Applitools Eyes"></a>
    <a href="https://github.com/webdriverio/webdriverrtc"><img src="/images/plugins/webrtc.png" alt="WebRTC Analytics Plugin"></a>
</div>


## What is WebdriverIO?
## WebdriverIO とは


<div style="overflow: hidden">
    <article class="col2">
        WebdriverIO を使うと、わずか数行のコードでブラウザまたはモバイルアプリケーションを制御できます。テストコードはシンプルで簡潔で読みやすくなります。統合されたテストランナーを使用すると、非同期コマンドを同期的に書くことができ、[競合状態](https://ja.wikipedia.org/wiki/%E7%AB%B6%E5%90%88%E7%8A%B6%E6%85%8B)を避けるために Promise での処理方法を気にする必要はありません。さらに、煩雑なセットアップ作業をすべて省き、Selenium セッションを管理します。
        <br>
        <br>
        ページ上の要素を操作することは、その同期性のためにこれまでは簡単ではありませんでした。要素をフェッチまたはループするときは、ネイティブの JavaScript 関数だけを使用できます。`$` および `$$` 関数を使うと、WebdriverIO は便利なショートカットを提供し、複雑な xPath セレクタを使用せずに DOM ツリーのより深いところへ移動することができます。
        <br>
        <br>
        テストランナーには、エラーが発生した場合にスクリーンショットを取るなど、テストプロセスに干渉したり、以前のテスト結果に従ってテスト手順を変更するためのさまざまなフックが付属しています。 これは WebdriverIO の[サービス](/guide/services/appium.html) to integrate your tests with 3rd party tools like [Appium](http://appium.io/)が Appium などのサードパーティのツールとテストを統合するために使用されます。
    </article>

    <article class="runyourtests col2 last">
```js
var expect = require('chai').expect;
describe('webdriver.io api page', function() {
    it('should be able to filter for commands', function () {
        browser.url('http://webdriver.io/api.html');

        // filtering property commands
        $('.searchbar input').setValue('getT');

        // get all results that are displayed
        var results = $$('.commands.property a').filter(function (link) {
            return link.isVisible();
        });

        // assert number of results
        expect(results.length).to.be.equal(3);

        // check out second result
        results[1].click();
        expect($('.doc h1').getText()).to.be.equal('GETTEXT');
    });
});
```
    </article>
</div>

<a href="/guide.html" class="button getstarted">Get Started</a>
<div class="testimonials"></div>

<div style="overflow: hidden">
    <article class="col2 standalone">
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

        // outputs:
        // "Title is: WebdriverIO (Software) at DuckDuckGo"
    })
    .end();
```
    </article>
    <article class="col2 last">
        <h2 class="right-col-heading">スタンドアローン パッケージとしての WebdriverIO</h2>
        <p>
            WebdriverIOは、柔軟でフレームワークにとらわれないように設計されています。テストの目的だけでなく、どのような状況でも適用できます。
            <br>
            <br>
            スクレイピング ツールとして使用すると、Webサイトのデータを自動化された方法で動的に取得したり、独自の自動化ライブラリに統合することができます。その代表的な例は、[Spectron](http://electron.atom.io/spectron/), [Chimp](https://chimp.readme.io/) もしくは [CodeceptJS](http://codecept.io/) です。
            <div>
                <p>
                    <a href="http://electron.atom.io/spectron/" style="margin-right: 15px"><img src="http://electron.atom.io/images/spectron-icon.svg" width="75" /></a>
                    <a href="https://chimp.readme.io/" style="margin-right: 15px"><img src="https://www.filepicker.io/api/file/C4MBXB4jQ6Ld9gII5IkF" /></a>
                    <a href="http://codecept.io/"><img src="http://codecept.io/images/cjs-base.png" width="80" /></a>
                </p>
            </div>
        </p>
    </article>
</div>

## 簡単なテストセットアップ

`wdio` コマンドラインインターフェイスには、わずか1分で設定ファイルを作成するための便利な設定ユーティリティが付属しています。また、フレームワークの適応、レポーター、サービスなどの利用可能なサードパーティパッケージをすべて提供し、それらをインストールします。

<div class="cliwindow">
![WDIO configuration utility](/images/config-utility.gif "WDIO configuration utility")
</div>

<div>
    <article class="col2">
        <h2>どんな風に動くの?</h2>
        <p>
            WebdriverIOは、node.js 用のオープンソーステストユーティリティです。お好みのBDDまたはTDDテストフレームワークを使って JavasCript で非常に簡単に selenium テストを書くことが可能になります。
        </p>
        <p>
            基本的に<a href="https://www.w3.org/TR/webdriver/">WebDriver プロトコル</a>を介して Selenium サーバーにリクエストを送信し、そのレスポンスを処理します。これらのリクエストは便利なコマンドでラップされ、サイトをいくつかの観点から、自動化された方法でテストするために使用できます。
        </p>
    </article>

    <article class="runyourtests col2 last">
        <h2>クラウドでテストを実行する</h2>
        <p>
            Sauce LabsやBrowserStackのようなサービスはリモートホスト上で selenium テストを提供します。環境内で設定を行わなくても、幅広いプラットフォーム、デバイス、ブラウザの組み合わせでテストを実行できます。
        </p>
        <div>
            <p>
            WebdriverIO は以下を含むサービスをサポートします：
            </p>
            <p>
                <a href="https://saucelabs.com">Sauce Labs</a>
                <a href="http://www.browserstack.com/">BrowserStack</a>
                <a href="http://www.testingbot.com/">TestingBot</a>
            </p>
        </div>
    </article>
</div>
