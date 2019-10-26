# 第8章: 並列実行とチャンネル

第1章を思い出してみましょう。並列実行のHello, World!を行いました。

以下のような感じです。

```ruby
channel = Channel(String).new
10.times do
  spawn {
    channel.send "Hello?"
  }
  puts channel.receive
end
```

Crystalでは、`spawn` キーワードを使って、メイン関数の実行をブロックすることなくバックグラウンドで何かを実行できます。

これを実現するために、`spawn`では `Fiber` という軽量スレッドを生成しています。`Fiber` はとても低コストに生成することができ、シングルコア上に数万のスレッドを容易に作ることができます。

`spawn` を使うことで、`Fiber` が裏で何かやっているのを気にすることなく、バックグラウンドで何かを実行できます。

次は、`チャンネル` の出番です。

## チャンネル <a id="channel"></a>

`Channel` の名前が示すとおり、チャンネルには送信側と受信側があります。なので、`Channel` はお互いと `send` と `receive` 関数を使ってやりとりをします。

先ほどの例を一行ずつ見ていきましょう。

```ruby
channel = Channel(String).new
```

チャンネルを `Channel(String).new` で作っています。`send` ならびに `receive` メソッドでやり取りする際、`String` 型を使って行うように作ったことを思い出してください。

```ruby
10.times do
  spawn {
    channel.send "Hello?"
  }
  puts channel.receive
end
```

ループする際、`spawn` の中でチャンネルにメッセージを送っています。「なぜメッセージをバックグラウンドで送る必要があるのか？」とお考えかもしれません。`send` は処理をブロックするので、メインプログラム内でこれを実行してしまうと、処理をブロックし続けてしまうのです。

これを踏まえて、以下のように実行してみます。

```ruby
channel = Channel(String).new
channel.send "Hello?" # This blocks the program execution
puts channel.receive
```

何か出力はありましたでしょうか。実際には、このプログラムは処理が終わりません。`channel.send "Hello?"` によって処理がブロックされてしまうためです。処理を続けるために、`spawn` を使う必要があるということが分かりました。

```ruby
spawn {
  channel.send "Hello?"
}
puts channel.receive
```

`spawn` を使ってバックグラウンドでチャンネルにメッセージを送信しました。これを、`channel.receive` を用いて受け取ります。この例ではメッセージは `Hello?` なので、これを表示してからプログラムが終了します。
