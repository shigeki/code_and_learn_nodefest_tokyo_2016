<!-- # How to write a test for the Node.js project -->
# Node.js プロジェクトにおけるテストの書き方

(この日本語訳は 2016/10/16 時点の https://github.com/nodejs/node/blob/v6.x/doc/guides/writing_tests.md の翻訳です。)

<!-- ## What is a test? -->
## テストとは？

<!--
A test must be a node script that exercises a specific functionality provided
by node and checks that it behaves as expected. It should return 0 on success,
otherwise it will fail. A test will fail if:
-->
テストは、Nodeによって提供される特定の機能を実行し、期待通りの挙動をすることをチェックするNodeスクリプトでなければ なりません。テストが成功したら 0 を返し、失敗するなら fail させなければなりません。テストは、次のような場合に fail します。

<!--
- It exits by calling `process.exit(code)` where `code != 0`
- It exits due to an uncaught exception.
- It never exits. In this case, the test runner will terminate the test because
  it sets a maximum time limit.
-->
- `code != 0` で `process.exit(code)` を呼び出して exit する。
- uncaught exception で exit する。
- exit しない。この場合、最大制限時間を設定しているので test runner がテストを終了します。

<!-- Tests can be added for multiple reasons: -->
幾つかの理由でテストを追加します。

<!--
- When adding new functionality.
- When fixing regressions and bugs.
- When expanding test coverage.
-->
- 新規機能を追加する時
- regressionやバグを修正する時
- テストカバレッジを広げる時

<!-- ## Test structure -->
## テスト構造

<!-- Let's analyze this very basic test from the Node.js test suite: -->
Node.jsのテストスィートから以下の基本的なテストを解説しましょう。

```javascript
1  'use strict';
2  const common = require('../common');
3  const http = require('http');
4  const assert = require('assert');
5
6  const server = http.createServer(common.mustCall((req, res) => {
7    res.end('ok');
8  }));
9  server.listen(0, () => {
10   http.get({
11     port: server.address().port,
12     headers: {'Test': 'Tokyo'}
13   }, common.mustCall((res) => {
14     assert.equal(res.statusCode, 200);
15     server.close();
16   }));
17 });
```

<!-- **Lines 1-2** -->
**1-2行目**

```javascript
'use strict';
const common = require('../common');
```

<!--
These two lines are mandatory and should be included on every test.
The `common` module is a helper module that provides useful tools for the tests.
If for some reason, no functionality from `common` is used, it should still be
included like this:
-->
これらの2つの行は必須です。全てのテストに含まれないといけません。
`common` モジュールは、テストのための便利なツールを提供するヘルパーモジュールです。
もし何らかの理由で `common` の機能を使わなくても、このように include すべきです。

```javascript
require('../common');
```

<!-- Why? It checks for leaks of globals. -->
なぜ？ global のリークをチェックするためです。

<!-- **Lines 3-4** -->
**3-4行目**

```javascript
const http = require('http');
const assert = require('assert');
```

<!--
These modules are required for the test to run. Except for special cases, these
modules should only include core modules.
The `assert` module is used by most of the tests to check that the assumptions
for the test are met.
-->
これらのモジュールは、テストを実行するために必要なものです。特別な場合を除いて、コアモジュールだけを include すべきです。
`assert` モジュールはテストの前提が合致しているのをチェックするもので、ほとんどのテストで使われます。

<!-- **Lines 6-17** -->
**6-17行目**

<!--
This is the body of the test. This test is quite simple, it just tests that an
HTTP server accepts `non-ASCII` characters in the headers of an incoming
request. Interesting things to notice:
-->
この部分はテスト本体です。このテストは非常に簡単なもので、HTTPサーバがリクエストを受け取る時にヘッダに non-ASCIIキ ャラクターを受け入れることをテストするだけのものです。注目すべき面白い点は、

<!--
- If the test doesn't depend on a specific port number then always use 0 instead
  of an arbitrary value, as it allows tests to be run in parallel safely, as the
  operating system will assign a random port. If the test requires a specific
  port, for example if the test checks that assigning a specific port works as
  expected, then it is ok to assign a specific port number.
- The use of `common.mustCall` to check that some callbacks/listeners are
  called.
- The HTTP server is closed once all the checks have run. This way, the test can
  exit gracefully. Remember that for a test to succeed, it must exit with a
  status code of 0.
-->
- もしテストが特定のポート番号に依存していないなら、任意の値を使うより 0 を常に使いましょう。なぜならOSがランダムポートを割り当てるので安全にテストを並行稼動させることができるからです。例えばテストが特定のポートに期待通りに割り当 てられていることをチェックするテストが必要なら、特定のポート番号を割り当てるのはOKです。
- `common.mustCall` は、あるコールバックやリスナーが呼ばれたことをチェックするために使います。
- HTTPサーバは全てのチェックが実行されたら一度クローズします。この方法で、テストは gracefully に exit できます。テ ストが成功したら status code 0 で exit しなければならないことを忘れてはいけません。

<!-- ## General recommendations -->
## 一般的な推奨

<!-- ### Timers -->
### タイマー

<!--
The use of timers is discouraged, unless timers are being tested. There are
multiple reasons for this. Mainly, they are a source of flakiness. For a thorough
explanation go [here](https://github.com/nodejs/testing/issues/27).
-->
タイマー自身のテスト以外でタイマーの利用は推奨されていません。これには様々な理由があります。
主に flakiness の原因となるからです。詳しくは、[ここ](https://github.com/nodejs/testing/issues/27)の説明をお読みく ださい。

<!--
In the event a timer is needed, it's recommended using the
`common.platformTimeout()` method, that allows setting specific timeouts
depending on the platform. For example:
-->
タイマーが必要なイベントでは、プラットフォームに依存した特定のタイムアウトを指定できる `common.platformTimeout()`  メソッドを使うことが推奨されます。例えば、

```javascript
const timer = setTimeout(fail, common.platformTimeout(4000));
```

<!--
will create a 4-seconds timeout, except for some platforms where the delay will
be multiplied for some factor.
-->
は、4秒タイムアウトを生成しますが、他のプラットフォームではある係数をかけた遅延になるでしょう。

<!-- ### The *common* API -->
### *common* API

<!--
Make use of the helpers from the `common` module as much as possible.
-->
`common` モジュールのヘルパーをできるだけ利用しましょう。

<!--
One interesting case is `common.mustCall`. The use of `common.mustCall` may
avoid the use of extra variables and the corresponding assertions. Let's explain
this with a real test from the test suite.
-->
`common.mustCall` が重要です。`common.mustCall` を利用すると、特別な変数やそれに対応した assertion を避けることができるでしょう。テストスイートからの実際のテストで説明しましょう。

```javascript
'use strict';
var common = require('../common');
var assert = require('assert');
var http = require('http');

var request = 0;
var response = 0;
process.on('exit', function() {
  assert.equal(request, 1, 'http server "request" callback was not called');
  assert.equal(response, 1, 'http request "response" callback was not called');
});

var server = http.createServer(function(req, res) {
  request++;
  res.end();
}).listen(0, function() {
  var options = {
    agent: null,
    port: this.address().port
  };
  http.get(options, function(res) {
    response++;
    res.resume();
    server.close();
  });
});
```

<!-- This test could be greatly simplified by using `common.mustCall` like this: -->
`common.mustCall` を使うことによってこのテストは次のように非常に簡略化することができるでしょう。

```javascript
'use strict';
var common = require('../common');
var assert = require('assert');
var http = require('http');

var server = http.createServer(common.mustCall(function(req, res) {
  res.end();
})).listen(0, function() {
  var options = {
    agent: null,
    port: this.address().port
  };
  http.get(options, common.mustCall(function(res) {
    res.resume();
    server.close();
  }));
});

```

<!-- ### Flags -->
### フラグ

<!--
Some tests will require running Node.js with specific command line flags set. To
accomplish this, a `// Flags: ` comment should be added in the preamble of the
test followed by the flags. For example, to allow a test to require some of the
`internal/*` modules, the `--expose-internals` flag should be added.
A test that would require `internal/freelist` could start like this:
-->
いくつかのテストは特定のコマンドラインフラグをセットして Node.jsを実行することが必要でしょう。
これを行うには、 `// Flags: ` コメントをテストの前文に加えてテストをその後に続けます。
例えば `internal/*` モジュールが必要なテストを行えるようにするには、`--expose-internals` フラグが必要です。
`internal/freelist` を必要とするテストは、このように始めます。

```javascript
'use strict';

// Flags: --expose-internals

require('../common');
const assert = require('assert');
const freelist = require('internal/freelist');
```