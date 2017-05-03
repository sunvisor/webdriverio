layout: api
name: api
title: WebdriverIO - API Docs
---

# WebdriverIO API ドキュメント

WebdriverIO のドキュメントページへようこそ。これらのページには、実装されているすべての selenium のバインディングとコマンドのリファレンス資料が含まれています。 WebdriverIO はすべての [JSONWire protocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) プロトコルコマンドを実装しており、[Appium](http://appium.io) の特別なバインディングもサポートしています。

## Examples

各コマンドのドキュメントには、通常、コマンドを同期して実行する WebdriverIO のテストランナーを使用したサンプルの使用例が示されています。WebdriverIO をスタンドアロンモードで実行する場合でも、すべてのコマンドを使用できますが、コマンドを連鎖させて Promise チェーンを解決して、実行順序が適切に処理されるようにする必要があります。したがって、変数に直接値を代入しないで、wdio testrunner を使うこともできます。

```js
it('can handle commands synchronously', function () {
    var value = browser.getValue('#input');
    console.log(value); // outputs: some value
});
```

promise が解決したときに値にアクセスするだけでなく、適切に解決されるように promise を返す必要があります。

```js
it('handles commands as promises', function () {
    return browser.getValue('#input').then(function (value) {
        console.log(value); // outputs: some value
    });
});
```

もちろん、Node.JSの最新の[async/await](https://github.com/yortus/asyncawait)機能を使用して、次のようにテストフローに同期構文を持たせることができます。

```js
it('can handle commands using async/await', async function () {
    var value = await browser.getValue('#input');
    console.log(value); // outputs: some value
});
```

しかし、テストランナーを使ってテストスイートをスケールアップすることをお勧めします。[Sauce Service](http://webdriver.io/guide/services/sauce.html) のような有用なアドオンが多数付属しているため、多くの定型コードを自分で書かずにすみます。

## エレメントは第一級オブジェクト

コードをより読みやすく、書くのを容易にするために、エレメント呼び出しを最初の第一級オブジェクトとして扱います。つまり、エレメントをクエリするために [`element`](/api/protocol/element.html) を呼び出すと、WebdriverIOはそのプロトタイプを結果に伝搬し、別のコマンドをチェーンすることができます。
[ページ オブジェクト パターン](http://martinfowler.com/bliki/PageObject.html)を使って、テストからアクセスできるように、ページ オブジェクトにページ エレメントを格納するようなテストを記述するときに便利です。

簡略化すると次のようになります。


```js
it('should use elements as first citizen', function () {
    var input = browser.element('.someInput');

    input.setValue('some text');
    // is the same as calling
    browser.setValue('.someInput', 'some text');
});
```

する各コマンドはセレクタを第1引数とし、セレクタを何度も何度も通過することなく実行できます。 これは見た目がよいだけでなく、同じ要素を何度も繰り返し照会することも避けています。スタンドアロンモードでも同じことができます：

```js
it('should use elements as first citizen in standalone mode', function () {
    return browser.element('.someInput').setValue('some text');
});
```

## Contribute

If you feel like you have a good example for a command, don't hesitate to open a PR and submit it. Just click on the orange button on the top right with the label _"Improve this doc"_. Make sure you understand the way we write these docs by checking the [Contribute](/contribute.html) section.
