# 第1章: Crystal を使いましょう

もしあなたが Ruby を使ってソフトウェア開発を行っているなら、 Crystal も使ってみてはいかがでしょうか。

Ruby のことを振り返ると、以下のような弱点があります。

* 並列処理
* 実行速度
* （英語の）ドキュメント不足

一方、Ruby の長所は以下のような点です。

* ブロック
* なんとなく書けて、動く
* 簡単な文法
* 開発しやすさに重きを置いている
* すぐ書いてすぐ実行できる
* 動的型付け言語であること

Ruby と同じくらい簡単に学習できて、だけれども並列処理をしっかりできて、無名関数や複雑な文法や`AbstractFactoryFactoryImpls`なんてクラスを作らなくて済む言語。

それが _Crystal_ です。

今はまだ Crystal も完璧ではありません。しかし、「学びやすさ」という点では、より親しみのある言語を使う方が、他の言語より学びが多いと考えています。

Crystal での “Hello World” を実行してみましょう。

```ruby
puts "Hello, world!"
```

次は、並列実行版の “Hello World” です。

```ruby
channel = Channel(String).new
10.times do
  spawn {
    channel.send "Hello?"
  }
  puts channel.receive
end
```

ざっくり Ruby に書き直すと、次のようになります。

```ruby
10.times.map do
  Thread.new do
    puts "Hello?"
  end
end.each(&:join)
```

下記の点で Ruby とよく似ていることがわかるかと思います。

* ほぼ同じ文法
* 変数は静的型付け言語ですが、型推論があるため、型を宣言する必要がないこと

違いは、以下の点です。

* コンパイル言語であること
* 静的型付け言語であること

実行速度を見てみると…

```text
$ time ./hello
./hello  0.00s user 0.00s system 73% cpu 0.008 total

$ time ruby hello.rb
ruby hello.rb  0.03s user 0.01s system 94% cpu 0.038 total
```

5倍も速い（笑）微小な差ではありますが。

いずれにせよ強調したい点としては、Crystal は Ruby とほぼ同じ文法で扱いやすいという点です。一方で、強みは Ruby の弱点を補っているところです。Crystal を通じて得られるものが多いと言えます。
