name: Gulp
category: plugins
tags: guide
index: 0
title: WebdriverIO - gulp-webdriver
---

gulp-webdriver
==============

`gulp-webdriver` は、 [WebdriverIO](http://webdriver.io) テストランナーで selenium テストを実行する [gulp プラグイン](http://gulpjs.com/)です。

## インストール

```shell
npm install gulp-webdriver --save-dev
```

## 使用法

You can run WebdriverIO locally running this simple task:
次の単純なタスクをローカルで実行すると、WebdriverIO をローカルで実行できます。

```js
gulp.task('test:e2e', function() {
    return gulp.src('wdio.conf.js').pipe(webdriver());
});
```

gulp-webdriver は wdio テストランナーに簡単にアクセスできるようにし、複数の設定ファイルを順番に実行できるようにします。必要に応じて、wdio コマンドに追加の引数を渡して、テストを指定することができます。[ここ](http://webdriver.io/guide/testrunner/gettingstarted.html)で利用可能なすべてのオプションを見つけるか、 `$ wdio --help`  (WebdriverIOがグローバルにインストールされている場合) を実行してください。

```js
gulp.task('test:e2e', function() {
    return gulp.src('wdio.conf.js').pipe(webdriver({
        logLevel: 'verbose',
        waitforTimeout: 10000
    }));
});
```
