name: bindingscommands
category: usage
tags: guide
index: 2
title: WebdriverIO - Bindings & Commands
---

バインディングとコマンド
=====================

WebdriverIOは、プロトコル バインディングとコマンドという2つの異なるメソッドタイプを区別します。プロトコル バインディングは、JSONWire プロトコルインターフェイスの正確な表現です。 それらは、[プロトコル文書](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol)で説明されているのと同じパラメータを期待します。

```js
console.log(browser.url());
 /**
  * returns:
  *
  * { state: null,
  *   sessionId: '684cb251-f0bd-4910-a43d-f3413206e652',
  *   hCode: 2611472,
  *   value: 'http://webdriver.io',
  *   class: 'org.openqa.selenium.remote.Response',
  *   status: 0 } }
  *
  */
});
```

WebdriverIOは、実装されたすべてのJSONWireプロトコルバインディング（Webdriverスペックの新しいものを含む）をサポートしています。さらに、モバイルテスト用の[Appium](http://appium.io/) 固有のものもあります。

All other methods like `getValue` or `dragAndDrop` using these commands to provide handy functions who simplify your tests to look more concise and expressive. So instead of doing this:

これらのコマンドを使用して`getValue` や `dragAndDrop` などの他のすべてのメソッドを使用すると、テストを簡略化してより簡潔で表現力豊かな便利な関数を提供できます。 ですからこの代わりに、


```js
var element = browser.element('#myElem');
var res = browser.elementIdCssProperty(element.value.ELEMENT, 'width');
assert(res.value === '100px');
});
```

シンプルにこうすることができます。

```js
var width = browser.getCssProperty('#myElem', 'width')
assert(width.parsed.value === 100);
/**
 * console.log(width) returns:
 *
 * { property: 'width',
 *   value: '100px',
 *   parsed: { type: 'number', string: '100px', unit: 'px', value: 100 } }
 */
});
```

コードを短くするだけでなく、戻り値をテスト可能にします。特定のアクションを達成するために JSONWire バインディングをまとめる方法を心配する必要はありません。単にコマンドメソッドを使用します。

WebdriverIO コマンドを呼び出すと、自動的にプロトタイプを戻り値に反映しようとします。これはもちろん、戻り値のプロトタイプを変更できる場合（オブジェクトなど）にのみ機能します。
これにより、開発者は次のようにコマンドをチェーンさせることができます。

```js
browser.click('#elem1').click('#elem2');
```

また、戻り値のエレメントに対してコマンドを呼び出すこともできます。browser インスタンスは、各コマンドの最後の戻り値を記憶し、次のコマンドのパラメータとしてそれを伝播することができます。この方法で、戻り値のエレメントに対して直接コマンドを呼び出すことができます。

```html
<div id="elem1" onClick="document.getElementById('elem1').innerHTML = 'some new text'">some text</div>
```

```js
var element = browser.element('#elem1');
console.log(element.getText()); // outputs: "some text"
element.click();
console.log(element.getText()); // outputs: "some new text"
```

このメソッドを使用すると、ページ情報を[page オブジェクト](http://webdriver.io/guide/testrunner/pageobjects.html)にシームレスにカプセル化することができ、次のような非常に表現力豊かなテストを書くことができます。

```js
var expect = require('chai').expect;
var FormPage = require('../pageobjects/form.page');

describe('auth form', function () {
    it('should deny access with wrong creds', function () {
        FormPage.open();
        FormPage.username.setValue('foo');
        FormPage.password.setValue('bar');
        FormPage.submit();

        expect(FormPage.flash.getText()).to.contain('Your username is invalid!');
    });
});
```

[page オブジェクト](http://webdriver.io/guide/testrunner/pageobjects.html)のドキュメントページをチェックするか、 [サンプル](https://github.com/webdriverio/webdriverio/tree/master/examples/pageobject)をご覧ください。
