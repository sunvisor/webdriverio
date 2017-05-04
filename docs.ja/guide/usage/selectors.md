name: selectors
category: usage
tags: guide
index: 0
title: WebdriverIO - Selectors
---

セレクタ
=========

JsonWireProtocolは、エレメントをクエリするためのいくつかのストラテジーを提供します。 WebdriverIO はこれらを解りやすくするために[Sizzle](http://sizzlejs.com/) のような一般的な既存のセレクタライブラリで単純化します。 次のセレクタタイプがサポートされています。

## CSS クエリ セレクタ

```js
browser.click('h2.subheading a');
```

## リンク テキスト

特定のテキストを含む`A` エレメントを取得するには、`=` 記号で始まるテキストでクエリします。

```html
<a href="http://webdriver.io">WebdriverIO</a>
```

```js
console.log(browser.getText('=WebdriverIO')); // outputs: "WebdriverIO"
console.log(browser.getAttribute('=WebdriverIO', 'href')); // outputs: "http://webdriver.io"
```

## 部分一致

テキストが、検索値と部分一致するエレメントを探すには、クエリ文字列の先頭に `*=` をつけます。例: `*=driver`

```html
<a href="http://webdriver.io">WebdriverIO</a>
```

```js
console.log(browser.getText('*=driver')); // outputs: "WebdriverIO"
```

## 特定のテキストを持つエレメント

同じテクニックをエレメントにも適用することができます。たとえば、「Welcome to my Page」というテキストの H1 タグをクエリします。

```html
<h1>Welcome to my Page</h1>
```

```js
console.log(browser.getText('h1=Welcome to my Page')); // outputs: "Welcome to my Page"
console.log(browser.getTagName('h1=Welcome to my Page')); // outputs: "h1"
```

部分一致も使えます

```js
console.log(browser.getText('h1*=Welcome')); // outputs: "Welcome to my Page"
```

ID や class も同様です。

```html
<i class="someElem" id="elem">WebdriverIO is the best</i>
```
```js
console.log(browser.getText('.someElem=WebdriverIO is the best')); // outputs: "WebdriverIO is the best"
console.log(browser.getText('#elem=WebdriverIO is the best')); // outputs: "WebdriverIO is the best"
console.log(browser.getText('.someElem*=WebdriverIO')); // outputs: "WebdriverIO is the best"
console.log(browser.getText('#elem*=WebdriverIO')); // outputs: "WebdriverIO is the best"
```

## タグ名

特定のタグ名を持つ要素を照会するには、
`<tag>` or `<tag />` を使います。

## name 属性

指定した name 属性を持つエレメントをクエリするには、セレクタのパラメータに `[name="some-name"]` などと渡して、通常の CSS3 セレクタや JsonWireProtocol が提供する name ストラテジーを使用できます

## xPath

また、特定のxPath経由でエレメントをクエリすることもできます。セレクタは、例えば `//BODY/DIV[6]/DIV[1]/SPAN[1]` のような形式で指定します。

近い将来、WebdriverIO はフォームセレクタ（例 `:password` 、 `:file` など）や:first や :nth のような位置セレクタなどの、さらなるセレクタ機能を用意する予定です。

## Mobile Selectors

（ハイブリッド/ネイティブ）モバイルテストでは、モバイル ストラテジーを使用し、基礎となるデバイス自動化技術を直接使用する必要があります。これは、テストでエレメントの検索を細かく制御する必要がある場合に特に便利です。

### Android UiAutomator

AndroidのUI Automatorフレームワークは、エレメントを見つけるためのさまざまな方法を提供します。 [UI Automator API](https://developer.android.com/tools/testing-support-library/index.html#uia-apis) 、特に [UiSelector class](https://developer.android.com/reference/android/support/test/uiautomator/UiSelector.html) クラスを使用してエレメントを見つけることができます。Appiumでは、文字列としてJavaコードをサーバーに送信し、アプリケーションの環境でそれを実行し、エレメントを返します。

```js
var selector = 'new UiSelector().text("Cancel")).className("android.widget.Button")';
browser.click('android=' + selector);
```

### iOS UIAutomation

OSアプリケーションを自動化する場合、[AppleのUI Automation フレームワーク](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html)を使用してエレメントを見つけることができます。このJavaScript APIには、ビューやそのすべてにアクセスするためのメソッドがあります。

```js
var selector = 'UIATarget.localTarget().frontMostApp().mainWindow().buttons()[0]'
browser.click('ios=' + selector);
```

Appium の iOS UI Automation 内で述語検索を使用して、エレメントの検索をさらに制御することもできます。 詳細は[こちら](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/ios_predicate.md)を参照してください。

### Accessibility ID

`accessibility id` ロケータ ストラテジーは、UIエレメントの一意の識別子を読み取るように設計されています。これは、ローカリゼーションやテキストを変更する可能性のあるその他のプロセスでは変更されないという利点があります。さらに、機能的に同じエレメントが同じアクセシビリティIDを持つ場合、クロスプラットフォームテストの作成を支援することができます。

- iOSの場合、これはAppleが[ここ](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAccessibilityIdentification_Protocol/index.html)に示したaccessibility identifier です 。

- Androidの場合、[ここ](https://developer.android.com/training/accessibility/accessible-app.html)で説明するように、 accessibility idはエレメントのcontent-description記述に対応します 。

どちらのプラットフォームでもエレメントまたは複数のエレメントを取得する場合、 `accessibility id` が最適な方法です。また、`name` ストラテジーは非推奨なので、代わりにこの方法を推奨します。

```js
browser.click(`~my_accessibility_identifier`);
```

### Class Name

The `class name` strategy is a `string` representing a UI element on the current view.
`class name` ストラテジーは、現在のビューの UI エレメントを表す文字列 (`string`) です。

- iOSの場合は、 [UIAutomation class](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) クラスの完全な名前で、テキストフィールドの`UIATextField`など、`UIA` で始まります。完全なリファレンスは[ここ](https://developer.apple.com/library/ios/navigation/#section=Frameworks&topic=UIAutomation)にあります。

- Androidの場合は、テキストフィールド用の `android.widget.EditText` など、 [UI Automator](https://developer.android.com/tools/testing-support-library/index.html#UIAutomator) [class](https://developer.android.com/reference/android/widget/package-summary.html) の完全修飾名です。 完全なリファレンスは[ここ](https://developer.android.com/reference/android/widget/package-summary.html)にあります。

```js
// iOS example
browser.click(`UIATextField`);
// Android example
browser.click(`android.widget.DatePicker`);
```

## チェーン セレクタ

より具体的にクエリするには、適切なエレメントが見つかるまでセレクタをチェーンすることができます。実際のコマンドの前にエレメントを呼び出すと、WebdriverIO はそのエレメントからクエリを開始します。たとえば、次のようなDOM構造があるとします。

```html
<div class="row">
  <div class="entry">
    <label>Product A</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
  <div class="entry">
    <label>Product B</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
  <div class="entry">
    <label>Product C</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
</div>
```

ここで商品Bをカートに追加するには、CSSセレクターを使用するだけでは困難です。 セレクタチェインを使用すると、目的のエレメントを段階的に絞り込むことができます。

```js
browser.element('.row .entry:nth-child(1)').click('button*=Add');
```
