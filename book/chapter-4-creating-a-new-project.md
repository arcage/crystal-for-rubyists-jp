# 第4章: プロジェクトを作成しよう

これまでは、 `crystal` コマンドをコードを実行するためだけに使ってきました。

`crystal` コマンドにはそれだけではない、多くの便利な機能が備わっています。（`crystal --help` を確認してください）

たとえば、新しい Crystal プロジェクトを作成することもできます。

```text
$ crystal init app sample
  create  sample/.gitignore
  create  sample/LICENSE
  create  sample/README.md
  create  sample/.travis.yml
  create  sample/shard.yml
  create  sample/src/sample.cr
  create  sample/src/sample/version.cr
  create  sample/spec/spec_helper.cr
  create  sample/spec/sample_spec.cr
Initialized empty Git repository in /Users/serdar/crystal_for_rubyists/code/04/sample/.git/
```

素晴らしい。

`crystal` コマンドは新しいプロジェクトの生成を手助けしてくれます。実際に何をやっているのかを見てみましょう。

* 「sample」という名前のフォルダを作成
* LICENSE ファイルを作成
* Travisを使って容易にCIを実現するための `.travis.yml` を作成
* 依存関係管理のための `shard.yml` を作成
* 空の Git リポジトリを初期化
* README ファイルを作成
* ソースコードを置くための `src` フォルダ、 テストコードを置くための `spec` フォルダを作成（`spec`については後述します）

では、実行してみましょう。

```text
$ cd sample
$ crystal src/sample.cr
```

何もおきません（笑）

さて、最初のプロジェクトができました。次に、いくつか外部ライブラリを使用てみましょう。

## Shards をによる依存関係の管理 <a id="using-shards-for-dependency-management"></a>

プロジェクトの依存関係を管理するのに `shards` コマンドを使います。おおよそ、`shards` は `bundler` に、`shard.yml` は `Gemfile` に相当します。

`shard.yml` を開いてみましょう。

```text
name: sample
version: 0.1.0

authors:
  - sdogruyol <dogruyolserdar@gmail.com>

license: MIT
```

デフォルトの `shard.yml` には、以下のようなプロジェクトについて最低限の情報が記載されています。

* `name` はプロジェクト名を示します
* `version` はプロジェクトのバージョンを示しています。 Crystal 自体はバージョン管理に [semver](http://semver.org/) を使います。のちのち役立つでしょう。
* `authors` セクションはプロジェクトの著者を示します。デフォルトでは `git` の設定から名前を取得します。
* `license` はプロジェクトのライセンスを示します。デフォルトでは `MIT` ライセンスになります。

これはこれでよいとして、`shard.yml` を使って他に何ができるのでしょうか？ 例えば、このファイルに外部ライブラリ（依存ライブラリとも呼びます）についての記述を追加して、ファイルやパスなどを気にせずそれらを管理することができます。すごくない？

さて、あなたは今 `shards` の真の力を知りました。早速、我々の `shard.yml` に [Kemal](https://github.com/sdogruyol/kemal) を追加して、簡単なWebアプリケーションを作ってみましょう。

`shard.yml` を開きましょう。まず必要なのは `Kemal` をプロジェクトの依存ライブラリとして追加することです。そのためには、`shard.yml` に以下の内容を加えてください。

```text
dependencies:
  kemal:
    github: sdogruyol/kemal
    version: 0.14.1
```

すばらしい。これでプロジェクトに `Kemal` が追加されました。次は `Kemal` のインストールです。

```text
$ shards install
Updating https://github.com/sdogruyol/kemal.git
Updating https://github.com/luislavena/radix.git
Updating https://github.com/jeromegn/kilt.git
Installing kemal (0.14.1)
Installing radix (0.3.0)
Installing kilt (0.3.3)
```

これで `Kemal` を使う準備ができました。では、`src/sample.cr` を開きましょう。

```ruby
require "./sample/*"
require "kemal"

module Sample

  get "/" do
    "Hello World!"
  end

end

Kemal.run
```

どのように `Kemal` を `require` して、使っているか見てみてください。

では、実行してみましょう。

```text
$ crystal src/sample.cr
[development] Kemal is ready to lead at http://0.0.0.0:3000
```

Webブラウザで `localhost:3000` を開き、動作を確認してみてください。

これで、依存ライブラリを追加して、だれかが作った `shard` を使う方法がわかりましたね。
