# 第5章: テスト

Rubyist はテスト大好きなので、まずはテストについて触れましょう。Crystal ではテストフレームワークが組み込まれています。名前は `spec` と言って、`RSpec` とよく似ています。

第4章で作ったプロジェクトで続けます。

`crystal` で以下のようなプロジェクトの構造を作ったことを思い出してください。

```text
$ cd sample && tree
-- LICENSE
-- README.md
-- shard.yml
-- spec
  -- sample_spec.cr
  -- spec_helper.cr
-- src
  -- sample
    -- version.cr
  -- sample.cr
```

`spec` フォルダがあるのが分かります。お察しのとおり、Crystal が最初の spec を作ってくれています。Crystal ではソースファイルは対応する `_spec` ファイルでテストします。プロジェクト名を `sample` とつけたので、ソースファイルは `sample.cr` 、対応する spec ファイルは `spec/sample_spec.cr` となります。

本書では、`spec` と `unit test` を同じ意味で使っています。

`spec/sample_spec.cr` を開いてみましょう。

```ruby
require "./spec_helper"

describe Sample do
  # TODO: Write tests

  it "works" do
    false.should eq(true)
  end
end
```

3つの重要なキーワード、`describe`, `it` と `should` が記載されています。

これらのキーワードは `spec` ファイル内で次の役割で使われています。

* `describe` は spec が関係するグループを示します
* `it` は spec を定義するのに使います。spec のタイトルをダブルクオートで囲います
* `should` は spec の assumption を作るのに使います

見てのとおり、specファイルには以下が含まれています。

* `Sample` と名づけられたグループ
* タイトルに `works` と付けられているひとつの spec `it`
* spec の想定は、false が true と一致すべき \(`should`\) であることを示している

これらのテストを実行するためには、やはり `crystal` コマンドを使います。

```text
$ crystal spec
F

Failures:

  1) Sample works
     Failure/Error: false.should eq(true)

       expected: true
            got: false

     # ./spec/sample_spec.cr:7

Finished in 0.69 milliseconds
1 examples, 1 failures, 0 errors, 0 pending

Failed examples:

crystal spec ./spec/sample_spec.cr:6 # Sample works
```

テストが失敗 \(red\) しました。出力を読むと、どの spec が失敗したかがすぐに分かります。 `Sample` グループの `works` というタイトルの spec です。`Sample works` と記載されています。このテストを通して \(green\) みましょう。

```ruby
require "./spec_helper"

describe Sample do
  # TODO: Write tests

  it "works" do
    true.should eq(true)
  end
end
```

再度、実行してみましょう。

```text
$ crystal spec

.

Finished in 0.63 milliseconds
1 examples, 0 failures, 0 errors, 0 pending
```

成功しました。これが覚えるべきすべてです。次の章では、FizzBuzz を作ってみましょう。