name: transferpromises
category: usage
tags: guide
index: 5
title: WebdriverIO - Transfer Promises
---

Promises への移行
=================

デフォルトでは、wdio テストランナーはすべてのコマンドを実際の同期コマンドのように動作させます。この方法では、Promise を解決する必要はありません。しかし、wdio テストランナーを使用しない場合や、この動作を無効にしている場合、Promise 機能を使用して、約束どおりの約束などの約束ベースのアサーションライブラリで表現テストを記述することができます。
しかし、wdioテストランナーを使用しない場合や動作を無効にした場合、Promise のすばらしい機能を使用して[chai-as-promised](https://github.com/domenic/chai-as-promised/) などのPromise ベースのアサーションライブラリ) で表現的なテストを書くことができます。

```js
var client = require('webdriverio').remote({
    desiredCapabilities: {
        browserName: 'chrome'
    }
});
 
var chai = require('chai');
var chaiAsPromised = require('chai-as-promised');

chai.Should();
chai.use(chaiAsPromised);
chaiAsPromised.transferPromiseness = client.transferPromiseness;
 
describe('my app', function() {
    before(function () {
        return client.init();
    });
 
    it('should contain a certain text after clicking', function() {
        return client
            .click('button=Send')
            .isVisible('#status_message').should.eventually.be.true
            .getText('#status_message').should.eventually.be.equal('Message sent!');
    });
});
```

上記の例では、ユーザーが [送信] ボタンをクリックしてメッセージが送信される単純な統合テストを示しています。テストでは、ステータスメッセージが特定のテキストで表示されるかどうかを確認します。`transferPromiseness` 関数を WebdriverIO の内部通信者に設定することで、コマンドとともに連鎖させることができます。
