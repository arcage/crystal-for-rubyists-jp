# 第6章: FizzBuzz

Crystal でまずはじめにやることと言ったら、FizzBuzz を作ることでしょう。

ご存知ない方に補足すると、FizzBuzz は簡単なプログラミングの問題です。

> 「1から100までの数字を出力するプログラムを書け。ただし、3の倍数では数字の代わりに "Fizz" という文字列を、5の倍数では "Buzz" という文字列を、15の倍数では "FizzBuzz" という文字列を出力すること。」

この問題で、Crystal の基礎を学ぶことができます。ループ、テスト、標準出力への表示などです。

それでは、プロジェクトを作っていきましょう。

```text
$ crystal init app fizzbuzz
```

まずは失敗テストを書いていきます。`spec/fizzbuzz_spec.cr` を開きます。

```ruby
require "./spec_helper"

describe Fizzbuzz do
  it "shouldn't divide 1 by 3" do
    div_by_three(1).should eq(false)
  end
end
```

実行してみましょう。

```text
$ crystal spec
Error in ./spec/fizzbuzz_spec.cr:7: undefined method 'div_by_three'

div_by_three(1).should eq(false)
```

結果はもっともで、まだ何もメソッドを定義していません。これを作成します。

```ruby
require "./fizzbuzz/*"

def div_by_three(n)
  false
end
```

Ruby と同じく、最後に評価された値が戻り値として返ります。

TDD は最も単純なことをやるのが本質です。メソッドを定義したので、テストを実行してみましょう。

```text
$  crystal spec
.

Finished in 0.82 milliseconds
1 examples, 0 failures, 0 errors, 0 pending
```

テストが通りました。次のテストを書きましょう。

```ruby
require "./spec_helper"

describe Fizzbuzz do
  it "shouldn't divide 1 by 3" do
    div_by_three(1).should eq(false)
  end

  it "should divide 3 by 3" do
    div_by_three(3).should eq(true)
  end
end
```

実行します。

```ruby
$ crystal spec

.F

Failures:

  1) Fizzbuzz should divide 3 by 3
     Failure/Error: div_by_three(3).should eq(true)

       expected: true
            got: false

     # ./spec/fizzbuzz_spec.cr:9

Finished in 0.83 milliseconds
2 examples, 1 failures, 0 errors, 0 pending

Failed examples:

crystal spec ./spec/fizzbuzz_spec.cr:8 # Fizzbuzz should divide 3 by 3
```

またひとつ失敗しました。これを通します。

```ruby
require "./fizzbuzz/*"

def div_by_three(n)
  if n % 3 == 0
    true
  else
    false
  end
end
```

実行します。

```ruby
$ crystal spec

..

Finished in 0.61 milliseconds
2 examples, 0 failures, 0 errors, 0 pending
```

`else` 文がどのように動作するか分かります。これでも構いませんが、これを一行にリファクタリングしてみましょう。

私が書くとこうなります。

```ruby
def div_by_three(n)
  n % 3 == 0
end
```

最後に評価された値が返ることを思い出してください。

同様に、`div_by_five` 、`div_by_fifteen` メソッドを TDD で定義していきましょう。

```text
$ crystal spec -v

Fizzbuzz
  shouldn't divide 1 by 3
  should divide 3 by 3
  shouldn't divide 8 by 5
  should divide 5 by 5
  shouldn't divide 13 by 15
  should divide 15 by 15

Finished in 0.61 milliseconds
6 examples, 0 failures, 0 errors, 0 pending
```

それでは、メインとなるプログラムについて触れていきましょう。FizzBuzz を構成する道具はできました。これらを動かしてみましょう。まず必要なのは、1から100まで表示することです。

```ruby
100.times do |num|
  puts num
end
```

まずは、100回 **なにか** を表示します。`crystal build src/fizzbuzz.cr && ./fizzbuzz` で実行すると、 `num` を100回表示します。テストはまだ実行していないことに注意してください。

ふたつを合体させましょう。

```ruby
100.times do |num|
  answer = ""

  if div_by_fifteen num
    answer = "FizzBuzz"
  elsif div_by_three num
    answer = "Fizz"
  elsif div_by_five num
    answer = "Buzz"
  else
    answer = num
  end

  puts answer
end
```

`if` は値を返すので、以下のようにも書けます。

```ruby
(1..100).each do |num|
  answer = if div_by_fifteen num
    "FizzBuzz"
  elsif div_by_three num
    "Fizz"
  elsif div_by_five num
    "Buzz"
  else
    num
  end

  puts answer
end
```

ここで、`100.times` を `(1..100).each` に変更しました。0から99までの代わりに1から100までの `num` を作るためです。

実行してみましょう。これで FizzBuzz の完成です。
