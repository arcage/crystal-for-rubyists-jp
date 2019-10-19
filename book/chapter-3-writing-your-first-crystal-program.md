# 第3章: Crystal のプログラムを書いてみよう

## Crystal のプログラムを書いてみよう <a id="writing-your-first-crystal-program"></a>

それでは早速 “Hello, world” を書いてみましょう。`vim` でも何でもお好きなエディタでファイルを開きましょう。拡張子は `.cr` です。

```text
$ vim hello.cr
```

下記のように記述します。

```ruby
puts "Hello World!"
```

`Crystal` を実行してみましょう。

```text
$ crystal hello.cr
Hello World!
```

"Hello, World!" が出力されると思います。文字列がダブルクオートであることに注意してください。シングルクォートの場合は、以下のようなエラーになります。

```text
$ crystal hello.cr
Syntax error in ./hello.cr:1: unterminated char literal, use double quotes for strings

puts 'Hello World!'
```

`crystal` コマンドでの実行はすぐに実行できる一方、実行速度は遅くなっています。実行のたびにコンパイルが走っているためです。処理時間を計測してみましょう。

```text
$ time crystal hello.cr
crystal hello.cr  0.30s user 0.20s system 154% cpu 0.326 total
```

0.326 秒かかっています。`Hello World` にしては遅いです。

代わりに、`build` コマンドでコンパイルしてネイティブコードを生成してみましょう。

```text
$ crystal build hello.cr
```

通常の実行ファイルのように実行してみましょう。

```text
$ time ./hello
./hello  0.00s user 0.00s system 87% cpu 0.006 total
```

“Hello, world.” の出力が50倍も速くなりました。