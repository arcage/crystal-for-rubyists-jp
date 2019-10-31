# 第５章: テスト

Rubyist はテスが大好きです。ですので、なにはともあれテストについて語っていきましょう。Crysta にはRSpecとよく似た `spec` という名前のテストフレームワークがビルトインで用意されています。

第４章で作ったプロジェクトを続けて見ていきましょう

`crystal` コマンドが以下のようなプロジェクト構造を作成したのを覚えていますか？

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

`spec` フォルダがあるのに気づきましたか？ そうです。Crystal は初期 spec とともにこのフォルダを作ってくれています。Crysta では、ソースファイルはそれぞれの `_spec` ファイルを使ってテストします。先ほど `sample` という名前でプロジェクトつくり、その結果、ソースフィル名が `sample.cr` になっていますので、このファイルに対応する spec ファイル名は `spec/sample_spec.cr` になります。

ところで、ここでは「`spec`」という言葉を「ユニットテスト」と同じ意味で使用します。
By the way, in this context `spec` and `unit test` means the same so we can use them interchangeably.

では難しい話はおいておいて、`spec/sample_spec.cr` を開いてみましょう。

```ruby
require "./spec_helper"

describe Sample do
  # TODO: Write tests

  it "works" do
    false.should eq(true)
  end
end
```

さて、このファイルはかなり面白いですね。`describe`、`it`、`should` という３つの重要なキーワードがあります。

これらのキーワードは `spec` の中でしか使用しないもので、それぞれ以下の目的で使います。

* `describe`： 複数の関連した `spec` をグルーピングする
* `it`： 単体の `spec` に `""` で括ったタイトルを付けて定義する８
* `should`： その `spec` が想定する処理結果を指定する

このファイルには、`Sample` という軸でグルーピングされた `discribe` があり、その中には `works` というタイトルを持った `it` が１つ、さらにその中で「`false` は `true` と等しくなるはず（`.should eq()`）」という想定がなされています。

「どうやってテストを実行するの？」と聞かれるかもしれません。ここでも `crystal` コマンドが活躍します。

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

おっと、テストが失敗（red）してしまいました。テストの出力を読めば、どの `spec` が失敗したのかがすぐ分かります。`Sample` というグループの `works` というタイトルを持った `spec` （`Sample works` と表示されています）が失敗しているようですね。では、テストに合格（green）するようにしてみましょう。

```ruby
require "./spec_helper"

describe Sample do
  # TODO: Write tests

  it "works" do
    true.should eq(true)
  end
end
```

もう一度 `spec` を実行してみます。

```text
$ crystal spec

.

Finished in 0.63 milliseconds
1 examples, 0 failures, 0 errors, 0 pending
```

合格です!! ここまでが Crystal を使い始めるのに必要なことのすべてです。続いて FizzBuzz に挑戦してみましょう。

