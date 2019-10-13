# 第1章: なぜCrystalなのか?

皆さん、Rubyを使っていますか？なら話は簡単、Crystalも使ってみませんか？

Rubyについておさらいしましょう。Rubyの弱点は次の点と考えています。

* 並列性
* 実行速度
* (英語の)ドキュメント (訳注: 日本語のドキュメントは多い印象です)

Rubyのいいところはどこでしょう？

* ブロック
* なんとなく書いて動くところ
* 簡単な文法
* プログラマーの書きやすさに注力しているところ
* 起動と実行がすぐできるところ
* 動的型付け言語であるところ

Rubyと同じくらい簡単に覚えられて、うまく並列実行ができて、速い言語があったらと思いませんか。無名関数や、複雑な文法や、 `AbstractFactoryFactoryImpls` なんて作る必要に悩まされない言語。（訳注: ちょっと訳に不安があります。）

それが、_Crystal_ です。

今はまだ、Crystal は完璧じゃありません。発展途上です。しかし、ポイントは「学びやすさ」にあります。

Crystal の “Hello World” を見てみましょう。

```ruby
puts "Hello, world!"
```

次は、Crystal での並列実行の “Hello World” です。

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

以上です。Ruby とそっくりでしょ;)

* だいたい同じ文法
* 静的型付け言語ですが、型推論があるので、型を宣言する必要がありません

一方で「違い」は次の点です。

* コンパイル言語であること
* 静的型付け言語であること

```text
$ time ./hello
./hello  0.00s user 0.00s system 73% cpu 0.008 total

$ time ruby hello.rb
ruby hello.rb  0.03s user 0.01s system 94% cpu 0.038 total
```

五倍も速い！（あまり意味の無いベンチマークですが）

いずれにせよ、Crystal は文法が Ruby となんとなく似ています。そして、Ruby の弱点を補っているところを強調したいと思います。
