name: multiremote
category: usage
tags: guide
index: 4
title: WebdriverIO - Multiremote
---

複数のブラウザを同時に実行する
=====================================

WebdriverIO では、1度のテストで複数の Selenium セッションを実行できます。
これは、複数のユーザーを必要とするアプリケーション機能 (チャットや WebRTC アプリケーションなど) をテストする必要がある場合に便利です。
これらの各インスタンスで、[init](http://webdriver.io/api/protocol/init.html) や [url](http://webdriver.io/api/protocol/url.html) のような一般的なコマンドを実行する必要のあるリモートインスタンスを2つ作成するのではなく、1つのマルチリモート インスタンスを作成してすべてのブラウザを同時に制御することができます。
そうするには、`multiremote` 関数を使用して、ブラウザーに名前をつけ、その capabilities を値に持つオブジェクトを渡します。
各 capability に名前を付けることで、1つのインスタンスでコマンドを実行するときに、そのインスタンスを簡単に選択してアクセスすることができます。

スタンドアロンモードでマルチリモート WebdriverIO インスタンスを作成する方法を示す例が次になります。

```js
var webdriverio = require('webdriverio');
var browser = webdriverio.multiremote({
    myChromeBrowser: {
        desiredCapabilities: {
            browserName: 'chrome'
        }
    },
    myFirefoxBrowser: {
        desiredCapabilities: {
            browserName: 'firefox'
        }
    }
});
```

これにより、 Chrome と Firefox で2つの Selenium セッションが作成されます。
 Chrome や Firefox の代わりに、[Appium](http://appium.io/) を使用して2つのモバイルデバイスを起動することもできます。
ここでは、どのような種類のOS/ブラウザの組み合わせも可能です。
`browser` 変数を使用して呼び出すすべてのコマンドは、各インスタンスに対して並行して実行されます。
これは、統合テストを合理化し、実行を少しスピードアップするのに役立ちます。
たとえば、セッションを初期化してURLを開きます。


```js
browser.init().url('http://chat.socket.io/');
```

multiremote インスタンスを使用すると、コールバック関数でどのように結果にアクセスできるかを変更できます。複数のブラウザーがコマンドを実行するので、結果も複数受け取ります。

```js
browser
    .init()
    .url('https://www.whatismybrowser.com/')
    .getText('.string-major').then(function(result) {
        console.log(result.resultChrome); // 'Chrome 40 on Mac OS X (Yosemite)' が返る
        console.log(result.resultFirefox); // 'Firefox 35 on Mac OS X (Yosemite)' が返る
    })
    .end();
```

各コマンドが1つずつ実行されることに気づくでしょう。これは、すべてのブラウザが一度それを実行するとコマンドが終了することを意味します。これは、ブラウザーのアクションを同期させたままにし、現在起こっていることを理解しやすくするために役立ちます。

時には、テストのために各ブラウザで異なることをする必要がある場合があります。たとえば、チャットアプリケーションをテストする場合は、テキストメッセージを入力するブラウザが1つあり、他のブラウザではそのメッセージを受信してアサーションを行うのを待つ必要があります。`select` メソッドを使用すると、1つのインスタンスにアクセスできます。

```js
var myChromeBrowser = browser.select('myChromeBrowser');
var myFirefoxBrowser = browser.select('myFirefoxBrowser');
 
myChromeBrowser
    .setValue('#message', 'Hi, I am Chrome')
    .click('#send');
 
myFirefoxBrowser
    .waitForExist('.messages', 5000)
    .getText('.messages').then(function(messages) {
        assert.equal(messages, 'Hi, I am Chrome');
    });
```

この例では、`myChromeBrowser` インスタンスが送信ボタンをクリックすると、`myFirefoxBrowser` インスタンスがメッセージの待機を開始します。実行は並行して行われます。Multiremote は、簡単に複数のブラウザを同じように並行して操作したり、異なる操作をしたりできて、便利です。後者の場合、インスタンスを同期して何かを並行して実行したい場合があります。これを行うには、単に `sync` メソッドを呼び出します。`sync` メソッドの以降に連鎖するすべてのメソッドは、再び並列に実行されます。

```js
//これらのコマンドは、すべてのインスタンスで並列に実行されます
browser.init().url('http://example.com');
 
// Chromeブラウザで何かをする
myChromeBrowser.setValue('.chatMessage', 'Hey Whats up!').keys('Enter')
 
// Firefoxブラウザで何かをする
myFirefoxBrowser.getText('.message').then(function (message) {
    console.log(messages);
    // returns: "Hey Whats up!"
});
 
//もう一度インスタンスを同期させる
browser.sync().url('http://anotherwebsite.com');
```

これらのすべてのサンプルは、スタンドアロンモードでのマルチリモートの使用方法を示しています。もちろん、wdio テストランナーと併用することもできます。これを行うには、 `capabilities` オブジェクトを、ブラウザ名をキーにしたオブジェクトとして定義します。

```js
export.config = {
    // ...
    capabilities: {
        myChromeBrowser: {
            desiredCapabilities: {
                browserName: 'chrome'
            }
        },
        myFirefoxBrowser: {
            desiredCapabilities: {
                browserName: 'firefox'
            }
        }
    }
    // ...
};
```

すべてのコマンドがwdioテストランナーと同期して実行されているので、すべての multiremote コマンドも同期しています。 つまり、前述の `sync` メソッドは使いません。テスト スペックでは、各ブラウザはブラウザ名でグローバルに利用可能です：

```js
it('should do something with two browser', function () {
    browser.url('http://google.com');
    console.log(browser.getTitle()); // returns {myChromeBrowser: 'Google', myFirefoxBrowser: 'Google'}
 
    myFirefoxBrowser.url('http://yahoo.com');
    console.log(myFirefoxBrowser.getTitle()); // return 'Yahoo'
 
    console.log(browser.getTitle()); // returns {myChromeBrowser: 'Google', myFirefoxBrowser: 'Yahoo'}
});
```

__注:__ Multiremote はすべてのテストを並行して実行するものではありません。洗練された統合テストのために複数のブラウザーを調整するのに役立ちます。
