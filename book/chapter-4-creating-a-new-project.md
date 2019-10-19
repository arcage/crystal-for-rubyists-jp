# 第4章: プロジェクトを作成しよう

これまでは、 `crystal` コマンドをコードを実行するためだけに使ってきました。

`crystal` コマンドにはそれだけではない、多くの機能が備わっています。\(`crystal --help` を確認してください\)

例えば、新しいCrystalプロジェクトを作成することができます。

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

`crystal` は新しいプロジェクトを作成してくれます。何をやっているのかを見てみましょう。

* 「sample」という名前のフォルダを作成
* LICENSE ファイルを作成
* `.travis.yml` を作成し、TravisCIとの連携を容易にできます
* 依存関係管理のための `shard.yml` を作成
* 空の Git リポジトリで初期化
* README ファイルを作成
* ソースコードを置くための `src` フォルダ、 テストコードを置くための `spec` フォルダを作成 \(`spec`については後述します\)

実行してみましょう。

```text
$ cd sample
$ crystal src/sample.cr
```

何もおきません（笑）

それでは最初のプロジェクトを作っていきましょう。いくつか外部ライブラリを使用します。

## Shards を用いた依存関係の管理 <a id="using-shards-for-dependency-management"></a>

プロジェクトの依存関係を管理するのに `shards` を使います。`shards` は `bundler` に、`shard.yml` は `Gemfile` に相当します。

`shard.yml` の内容を見てみましょう。

```text
name: sample
version: 0.1.0

authors:
  - sdogruyol <dogruyolserdar@gmail.com>

license: MIT
```

デフォルトの `shard.yml` は上記のように、プロジェクトについての最低限の情報が記載してあります。

* `name` はプロジェクト名を示します
* `version` はプロジェクトのバージョンを示しています。 Crystal 自体はバージョン管理に [semver](http://semver.org/) を使います。のちのち役立つでしょう。
* `authors` セクションはプロジェクトの著者を示します。デフォルトでは `git` の設定から名前を取得します。
* `license` はプロジェクトのライセンスを示します。デフォルトでは `MIT` ライセンスになります。

`shard.yml` の役割について確認しました。このファイルに外部ライブラリを追加することができます。また、フォルダやパスなどを考慮することなく、依存関係を管理することができます。

`shards` の真の力をお見せしましょう。`shard.yml` に [Kemal](https://github.com/sdogruyol/kemal) ライブラリを追加します。簡単なウェブアプリをビルドしましょう。

`shard.yml` を開き、 `Kemal` を依存関係に追加します。次のように記載します。

```text
dependencies:
  kemal:
    github: sdogruyol/kemal
    version: 0.14.1
```

これで、`Kemal` ライブラリがプロジェクトに追加されました。これをインストールします。

```text
$ shards install
Updating https://github.com/sdogruyol/kemal.git
Updating https://github.com/luislavena/radix.git
Updating https://github.com/jeromegn/kilt.git
Installing kemal (0.14.1)
Installing radix (0.3.0)
Installing kilt (0.3.3)
```

これで `Kemal` を使う準備ができました。`src/sample.cr` を開きましょう。

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

どうやって `Kemal` を `require` して、使うかが分かります。

実行してみましょう。

```text
$ crystal src/sample.cr
[development] Kemal is ready to lead at http://0.0.0.0:3000
```

ブラウザで `localhost:3000` を開き、結果を確認してみてください。

これで、依存関係を追加して他の `shard` を使う方法がわかりました。
