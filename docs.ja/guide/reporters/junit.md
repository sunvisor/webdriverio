name: junit
category: reporters
tags: guide
index: 2
title: WebdriverIO - JUnit Reporter
---

JUnit Reporter
==============

JUnit レポーターは、CI サーバーの高度なレポートを作成するのに役立ちます。使用するには NPM からインストールするだけです。

```js
$ npm install wdio-junit-reporter --save-dev
```

次に、`wdio.conf.js` の `reporters` 配列に追加し、xml ファイルを格納するディレクトリを定義します。

```js
// wdio.conf.js
module.exports = {
    // ...
    reporters: ['spec', 'junit'],
    reporterOptions: {
        junit: {
            outputDir: './'
        }
    },
    // ...
};
```

WebdriverIOは各インスタンスに対して1つのレポートファイルを作成します。XML には、テストをデバッグするために必要なすべての情報が含まれます。

```xml
<!-- WDIO.xunit.safari.5_1.osx10_6.64814.xml -->
<testsuites name="safari-osx 10_6-5_1" tests="9" failures="0" errors="0" disabled="0" time="23.385">
  <testsuite name="webdriver.io page should have the right title" tests="3" failures="0" skipped="0" disabled="0" time="17.053" timestamp="Fri Jun 26 2015 14:19:37 GMT+0200 (CEST)" id="1" file="/Users/christianbromann/Sites/projects/webdriverio/DEV/examples/runner-specs/mocha.test.js">
      <testcase name="the good old callback way" disabled="false" time="9.848" id="4" file="/Users/christianbromann/Sites/projects/webdriverio/DEV/examples/runner-specs/mocha.test.js" status="passed">
          <system-out type="command"><![CDATA[
POST http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/url - {"url":"http://webdriver.io/"}
GET http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/title - {}
]]></system-out>
          <system-out type="result"><![CDATA[
POST http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/url - {"status":0,"orgStatusMessage":"The command executed successfully."}
GET http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/title - {"status":0,"state":null,"value":"WebdriverIO - WebDriver bindings for Node.js","sessionId":"3f1c00dc831f49698a50a793ca3049d9","hCode":1473790157,"class":"org.openqa.selenium.remote.Response"}
]]></system-out>
      </testcase>
      <testcase name="the promise way" disabled="false" time="3.656" id="7" file="/Users/christianbromann/Sites/projects/webdriverio/DEV/examples/runner-specs/mocha.test.js" status="passed">
          <system-out type="command"><![CDATA[
POST http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/url - {"url":"http://webdriver.io/"}
GET http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/title - {}
]]></system-out>
          <system-out type="result"><![CDATA[
POST http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/url - {"status":0,"orgStatusMessage":"The command executed successfully."}
GET http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/title - {"status":0,"state":null,"value":"WebdriverIO - WebDriver bindings for Node.js","sessionId":"3f1c00dc831f49698a50a793ca3049d9","hCode":139032836,"class":"org.openqa.selenium.remote.Response"}
]]></system-out>
      </testcase>
      <testcase name="the fancy generator way" disabled="false" time="3.549" id="9" file="/Users/christianbromann/Sites/projects/webdriverio/DEV/examples/runner-specs/mocha.test.js" status="passed">
          <system-out type="command"><![CDATA[
POST http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/url - {"url":"http://webdriver.io/"}
GET http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/title - {}
DELETE http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9 - {}
]]></system-out>
          <system-out type="result"><![CDATA[
POST http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/url - {"status":0,"orgStatusMessage":"The command executed successfully."}
GET http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9/title - {"status":0,"state":null,"value":"WebdriverIO - WebDriver bindings for Node.js","sessionId":"3f1c00dc831f49698a50a793ca3049d9","hCode":234367668,"class":"org.openqa.selenium.remote.Response"}
DELETE http://ondemand.saucelabs.com:80/wd/hub/session/3f1c00dc831f49698a50a793ca3049d9 - {"status":0,"sessionId":"3f1c00dc831f49698a50a793ca3049d9","value":""}
]]></system-out>
      </testcase>
  </testsuite>
</testsuites>
```

これにより、Jenkins のような CI システムとの優れた統合が可能になります。WebdriverIO と Jenkins を統合する方法の詳細については、[Jenkins Integration](http://webdriver.io/guide/testrunner/jenkins.html) のセクションをご覧ください。
