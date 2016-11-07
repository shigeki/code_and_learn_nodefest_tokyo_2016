<!-- # Contributing to Node.js -->
# Node.js への貢献

(この日本語訳は 2016/10/16 a4d396d85874046ffe6647ecb953fd78e16bcba3 時点の https://github.com/nodejs/node/blob/master/CONTRIBUTING.md の翻訳です。)

<!-- ## Code of Conduct -->
## 行動規範

<!--
The Code of Conduct explains the *bare minimum* behavior
expectations the Node Foundation requires of its contributors.
[Please read it before participating.](./CODE_OF_CONDUCT.md)
-->
行動規範は、Node Foundationが貢献者に求める必要最小限の振る舞いを解説したものです。始める前に[こちら](https://github.com/nodejs/node/blob/master/CODE_OF_CONDUCT.md)を読んで下さい。

<!-- ## Issue Contributions -->
## Issue 貢献

<!--
When opening new issues or commenting on existing issues on this repository
please make sure discussions are related to concrete technical issues with the
Node.js software.
-->
このレポジトリで新しい issue や既存の issue にコメントするときは、Node.js の具体的な技術的課題に関する議論であることを確かめてください。

<!--
For general help using Node.js, please file an issue at the
[Node.js help repository](https://github.com/nodejs/help/issues).
-->
Node.jsの利用に関する一般的な質問は、[Node.js help repository](https://github.com/nodejs/help/issues) に issue を出してく
ださい。

<!--
Discussion of non-technical topics including subjects like intellectual
property, trademark and high level project questions should move to the
[Technical Steering Committee (TSC)](https://github.com/nodejs/TSC/issues)
instead.
-->
知的財産権、登録商標や高レベルのプロジェクトの質問のような非技術的なト
ピックスの議論は、[Technical Steering Committee (TSC)](https://github.com/nodejs/TSC/issues) で行ってください。

<!-- ## Code Contributions -->
## コード貢献

<!--
The Node.js project has an open governance model and welcomes new contributors.
Individuals making significant and valuable contributions are made
_Collaborators_ and given commit-access to the project. See the
[GOVERNANCE.md](./GOVERNANCE.md) document for more information about how this
works.
-->
Node.jsプロジェクトはオープンガバナンスモデルを取っていて、新しい貢献者を歓迎しています。重要で価値のある貢献をしている個人は、_Collaborators_ になっていただき、プロジェクトへのコミット権限を与えられます。 このガバナンスがどうなっているのか、さらなる情報は[GOVERNANCE.md](https://github.com/nodejs/node/blob/master/GOVERNANCE.md) を見てください。

<!--
This document will guide you through the contribution process.
-->
この文書は、皆さんにコード貢献する方法を伝えるものです。

<!-- ### Step 1: Fork -->
### ステップ 1: フォーク

<!--
Fork the project [on GitHub](https://github.com/nodejs/node) and check out your
copy locally.
-->
[GitHub上](https://github.com/nodejs/node)のプロジェクトをフォークして、自分のローカルコピーにチェックアウトします。

```text
$ git clone git@github.com:username/node.git
$ cd node
$ git remote add upstream git://github.com/nodejs/node.git
```

<!-- #### Which branch? -->
#### どのブランチ？

<!--
For developing new features and bug fixes, the `master` branch should be pulled
and built upon.
-->
バグ修正や新機能の開発には、master ブランチを pull してビルドします。

<!-- #### Respect the stability index -->
#### stability indexを尊重する

<!--
The rules for the master branch are less strict; consult the
[stability index](./doc/api/documentation.md#stability-index) for details.
-->
masterブランチに対する規則は、それほど厳しくありません。詳細は、[stability index](https://github.com/nodejs/node/blob/master/doc/api/documentation.md#stability-index) を見てください。

<!--
In a nutshell, modules are at varying levels of API stability. Bug fixes are
always welcome but API or behavioral changes to modules at stability level 3
(Locked) are off-limits.
-->
簡単に書くと、モジュールは様々なAPI stabilityのレベルになっています。バグ修正はどのAPIに対しても常に歓迎されます。stabilityレベル3(Locked)のモジュールに対するAPIやその挙動の変更は、禁止されています。

<!-- #### Dependencies -->
#### 依存性

<!--
Node.js has several bundled dependencies in the *deps/* and the *tools/*
directories that are not part of the project proper. Any changes to files
in those directories or its subdirectories should be sent to their respective
projects. Do not send your patch to us, we cannot accept it.
-->
Node.jsは、*deps/* と *tools/* ディレクトリにプロジェクト固有でない依存ソフトウェアをバンドルして持っています。それらのディレクトリやサブディレクトリにあるファイルへの変更は、それぞれに対応するプロジェクトに送られるべきです。我々にパッチを送らないでください。受け入れることはできません。

<!--
In case of doubt, open an issue in the
[issue tracker](https://github.com/nodejs/node/issues/) or contact one of the
[project Collaborators](https://github.com/nodejs/node/#current-project-team-members).
Especially do so if you plan to work on something big. Nothing is more
frustrating than seeing your hard work go to waste because your vision
does not align with the project team. Node.js has two IRC channels,
[#Node.js](http://webchat.freenode.net/?channels=node.js) for general help and questions, and
[#Node-dev](http://webchat.freenode.net/?channels=node-dev) for development of node core specifically.
-->
わからない場合は、[issue tracker](https://github.com/nodejs/node/issues/) に issue を出すか、[project Collaborators](https://github.com/nodejs/node/#current-project-team-members)の一人に尋ねてください。特に何か大きな作業をするつもりなら問い合わせをしてください。あなたの大変な作業がプロジェクトチームのビジョンと合っていないために無駄になってしまう程もったいないことはありません。
Node.jsは2つのIRCチャンネルを持っています。一般的な質問やヘルプに対しては、
[#Node.js](http://webchat.freenode.net/?channels=node.js) へ、
特にNodeコアに開発に関することは、
[#Node-dev](http://webchat.freenode.net/?channels=node-dev) にお願いし
ます。

<!-- ### Step 2: Branch -->
### ステップ2: ブランチ

<!-- Create a branch and start hacking: -->
ブランチを作成してハッキングを始めます。

```text
$ git checkout -b my-branch -t origin/master
```

<!-- ### Step 3: Commit -->
### ステップ 3: コミット

<!-- Make sure git knows your name and email address: -->
git にあなたの名前と電子メールアドレスが登録されているか確認すること。

```text
$ git config --global user.name "J. Random User"
$ git config --global user.email "j.random.user@example.com"
```

<!--
Writing good commit logs is important. A commit log should describe what
changed and why. Follow these guidelines when writing one:
-->
良いコミットログを書くことは非常に重要です。コミットログは何をなぜ(what and why)変更したのか記載するべきです。コミットログを書くときには次に挙げるガイドラインに従います。

<!--
1. The first line should be 50 characters or less and contain a short
   description of the change. All words in the description should be in
   lowercase with the exception of proper nouns, acronyms, and the ones that
   refer to code, like function/variable names. The description should
   be prefixed with the name of the changed subsystem and start with an
   imperative verb, for example, "net: add localAddress and localPort
   to Socket".
2. Keep the second line blank.
3. Wrap all other lines at 72 columns.
-->
1. 最初の行は、50文字以下で変更点を短く記載したものを書きます。記載する文字は固有名詞、頭字語、関数や変数名などコードを参照するもの以外は全て小文字で書きます。変更するサブシステムの名前を文頭に記載し、続いて命令形にします。 例えば "net: add localAddress and localPort to Socket" の様に書きます。
2. 2行目は空行にします。
3. それ以外の行は全て72文字で改行して記載します。

<!-- A good commit log can look something like this: -->
良いコミットログは次のようなものです。

```txt
subsystem: explain the commit in one line

Body of commit message is a few lines of text, explaining things
in more detail, possibly giving some background about the issue
being fixed, etc. etc.

The body of the commit message can be several paragraphs, and
please do proper word-wrap and keep columns shorter than about
72 characters or so. That way `git log` will show things
nicely even when it is indented.
```

<!--
The header line should be meaningful; it is what other people see when they
run `git shortlog` or `git log --oneline`.
-->
ヘッダ行は意味ある記載にすべきです。他の人々が `git shortlog` や `git log --oneline` を実行したときに見るものになります。

<!--
Check the output of `git log --oneline files_that_you_changed` to find out
what subsystem (or subsystems) your changes touch.
-->
どのサブシステムをあなたが変更したか `git log --oneline あなたが変更したファイル` の出力でチェックしましょう。

<!--
If your patch fixes an open issue, you can add a reference to it at the end
of the log. Use the `Fixes:` prefix and the full issue URL. For example:
-->
もしあなたの修正が、オープンされている issue を修正するものなら、ログの最後にその issue への参照を追加しましょう。 `Fixes:` を頭に付けて、issueのURLを追加します。例えば、

```txt
Fixes: https://github.com/nodejs/node/issues/1337
```

<!-- ### Step 4: Rebase -->
### ステップ4: リベース

<!-- Use `git rebase` (not `git merge`) to sync your work from time to
time. -->
あなたの作業と同期する際は、毎回 `git rebase` を使いましょう。`git  merge` ではありません。

```text
$ git fetch upstream
$ git rebase upstream/master
```

<!-- ### Step 5: Test -->
### ステップ5: テスト

<!--
Bug fixes and features **should come with tests**. Add your tests in the
`test/parallel/` directory. For guidance on how to write a test for the Node.js
project, see this [guide](./doc/guides/writing_tests.md). Looking at other tests
to see how they should be structured can also help.
-->
バグ修正や新規機能追加は、必ず **テストをつける** べきです。
`test/parallel/` ディレクトリにテストを追加してください。 Node.jsにテストを各方法のガイドは、この [ガイド](./doc/guides/writing_tests.md)を見てください。他のテストがどういう風に作られているのか見ることも役に立つでしょう。

<!-- To run the tests on Unix / OS X: -->
UnixやOS Xでテストを実行するには、次の様にします。

```text
$ ./configure && make -j8 test
```

<!-- Windows: -->
Windowsでテストを実行するには、次の通りです。

```text
> vcbuild test
```

<!-- (See the [BUILDING.md](./BUILDING.md) for more details.) -->
(詳細は、 [BUILDING.md](./BUILDING.md) を見てください。)

<!--
Make sure the linter is happy and that all tests pass. Please, do not submit
patches that fail either check.
-->
linterが正常に完了し、全てのテストが通ることを確認してください。どちらかのチェックが失敗するようなパッチを送らないでください。

<!--
Running `make test`/`vcbuild test` will run the linter as well unless one or
more tests fail.
-->
`make test` や `vcbuild test` は、テストが失敗しなければ linter も実行します。

<!--
If you want to run the linter without running tests, use
`make lint`/`vcbuild jslint`.
-->
テストをせず linter だけ実行したい場合は、`make lint` や `vcbuild jslint` を行ってください。

<!--
If you are updating tests and just want to run a single test to check it, you
can use this syntax to run it exactly as the test harness would:
-->
もしテストを更新して一つのテストを実行して確認したいなら、テストハーネスとして次のやり方ができるでしょう。

```text
$ python tools/test.py -v --mode=release parallel/test-stream2-transform
```

<!-- You can run tests directly with node: -->
nodeを使って直接テストを実行することもできます。

```text
$ ./node ./test/parallel/test-stream2-transform.js
```
<!--
Remember to recompile with `make -j8` in between test runs if you change
core modules.
-->
コアモジュールを変更したらテストを実行する前に `make -j8` で再コンパイルすることを忘れないように。

<!-- ### Step 6: Push -->
### ステップ 6: プッシュ

```text
$ git push origin my-branch
```

<!--
Go to https://github.com/yourusername/node and select your branch.
Click the 'Pull Request' button and fill out the form.
-->
その後 https://github.com/yourusername/node に行って、あなたのブランチを選びます。'Pull Request' をクリックして、フォームの記載を埋めます。

<!--
Pull requests are usually reviewed within a few days. If there are comments
to address, apply your changes in a separate commit and push that to your
branch. Post a comment in the pull request afterwards; GitHub does
not send out notifications when you add commits.
-->
プルリクエストは通常数日以内にレビューされます。対処するコメントがあれば、別のコミットに変更を適応してあなたのブランチにプッシュしましょう。
その後プルリクエストにコメントしましょう。GitHubはコミットを追加しても通知をしてくれません。

<a id="developers-certificate-of-origin"></a>
## Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

* (a) The contribution was created in whole or in part by me and I
  have the right to submit it under the open source license
  indicated in the file; or

* (b) The contribution is based upon previous work that, to the best
  of my knowledge, is covered under an appropriate open source
  license and I have the right under that license to submit that
  work with modifications, whether created in whole or in part
  by me, under the same open source license (unless I am
  permitted to submit under a different license), as indicated
  in the file; or

* (c) The contribution was provided directly to me by some other
  person who certified (a), (b) or (c) and I have not modified
  it.

* (d) I understand and agree that this project and the contribution
  are public and that a record of the contribution (including all
  personal information I submit with it, including my sign-off) is
  maintained indefinitely and may be redistributed consistent with
  this project or the open source license(s) involved.


（訳者注)
DCOに関しては、 https://www.kernel.org/doc/Documentation/ja_JP/SubmittingPatches より和訳を引用します。

原作者の証明書( DCO ) 1.1

このプロジェクトに寄与するものとして、以下のことを証明する。

* (a) 本寄与は私が全体又は一部作成したものであり、私がそのファイル中に明示されたオープンソースライセンスの下で公開 する権利を持っている。もしくは、

* (b) 本寄与は、私が知る限り、適切なオープンソースライセンスでカバーされている既存の作品を元にしている。同時に、私 はそのライセンスの下で、私が全体又は一部作成した修正物を、ファイル中で示される同一のオープンソースライセンスで(異なるライセンスの下で投稿することが許可されている場合を除いて)投稿する権利を持って

* (c) 本寄与は(a)、(b)、(c)を証明する第3者から私へ直接提供されたものであり、私はそれに変更を加えていない。

* (d) 私はこのプロジェクトと本寄与が公のものであることに理解及び同意する。同時に、関与した記録(投稿の際の全ての個人情報と sign-off を含む)が無期限に保全されることと、当該プロジェクト又は関連するオープンソースライセンスに沿った形で再配布されることに理解及び同意する。