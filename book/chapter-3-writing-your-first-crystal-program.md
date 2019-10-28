# 第3章: Crystal のプログラムを書いてみよう

## 初めての Crystal のプログラムを書いてみよう <a id="writing-your-first-crystal-program"></a>

では始めましょう。自身をなにがしかの言語のプログラマーだと名乗るには、何はともあれその言語で `Hello World` プログラムを書かなければなりません。

まず、テキストファイルを開きまます。私は `vim` を使いますが、それは私が「そういう人」だからであって、それぞれが好きなエディタを使ってください。ちなみに、Crystalプログラムの拡張子は .cr です。

```text
$ vim hello.cr
```

エディタが開いたら以下を入力して保存します。

```ruby
puts "Hello World!"
```

そうしたら、`crystal` コマンドでプログラムを実行してみましょう

```text
$ crystal hello.cr
Hello World!
```

特にエラーなく "Hello, World!" が出力されるはずで。もし以下のようなエラーが表示さるようであれば、文字列を括るのがダブルクオートであることを再度確認してください。

```text
$ crystal hello.cr
Syntax error in ./hello.cr:1: unterminated char literal, use double quotes for strings

puts 'Hello World!'
```

ところで、`crystal` コマンドはすぐに実行できる一方で、実行速度は遅くなっています。これは、実行のたびにコンパイルが走っているためです。では、このプログラムを実行するのにかかっている時間を計ってみましょう。

```text
$ time crystal hello.cr
crystal hello.cr  0.30s user 0.20s system 154% cpu 0.326 total
```

0.326 秒もかかっています。`Hello Wolrd` にしては遅いですね。

次に、`build` コマンドでをつけてコンパイルしてネイティブコードを生成してみましょう。

```text
$ crystal build hello.cr
```

出来上がった実行ファイルを UNIX のコマンドの用に実行してみます。

```text
$ time ./hello
./hello  0.00s user 0.00s system 87% cpu 0.006 total
```

“Hello, world.” の出力が50倍も速くなったのを確認できるはずです。
