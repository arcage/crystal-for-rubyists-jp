# 第7章: 型と関数オーバーロード

Crystal は Ruby と似ていますが、Ruby とは違います。

Ruby とは違って、Crystal は静的型付け言語で、コンパイル型言語です。ほとんどの場合、型を宣言する必要はなく、コンパイラがスマートに `型推論` をしてくれます。

型が必要なケースを、簡単な例で見てみましょう。

```ruby
def add(x, y)
  x + y
end

add 3, 5 # 8
```

これは Ruby と同じです。ふたつの数字を足し合わせるメソッドを定義しました。これで、数字と文字列を足し合わせるとどうなるでしょう？

```ruby
add 3, "Serdar"
```

まず、Ruby で実行してみましょう。

```text
types.cr:2:in `+': String can't be coerced into Fixnum (TypeError)
    from types.cr:2:in `add'
    from types.cr:5:in `<main>'
```

`TypeError` が返りましたが、Ruby では型を考慮する必要はなかったはずです。これは `実行時エラー` とも呼ばれ、プログラムが実行時にクラッシュしたことを意味します。

同じことを Crystal で実行してみましょう。

```text
Error in ./types.cr:5: instantiating 'add(Int32, String)'

add 3, "Serdar"
^~~

in ./types.cr:2: no overload matches 'Int32#+' with types String
Overloads are:
- Int32#+(other : Int8)
- Int32#+(other : Int16)
- Int32#+(other : Int32)
- Int32#+(other : Int64)
- Int32#+(other : UInt8)
- Int32#+(other : UInt16)
- Int32#+(other : UInt32)
- Int32#+(other : UInt64)
- Int32#+(other : Float32)
- Int32#+(other : Float64)
- Int32#+()

x + y
  ^
```

出力にはだいぶビビりますが、Crystal ではコードはコンパイルされず、「`Int32#+` をオーバーロードする関数が無い」こと、代わりとなりそうなオーバーロードを教えてくれます。これは `コンパイル時エラー` であり、プログラムを実行する前にエラーを検知してくれたことを示しています。

次に、型を追加してメソッドが `Number` 型だけを受け取るように制限してみましょう。

```ruby
def add(x : Number, y : Number)
  x + y
end

puts add 3, "Serdar"
```

実行します。

```ruby
Error in ./types.cr:5: no overload matches 'add' with types Int32, String
Overloads are:
- add(x : Number, y : Number)

puts add 3, "Serdar"
     ^~~
```

また、コンパイルできませんでした。今回はエラー文が短く、より詳しくなっています。`x` と `y` に `型の制約` を設けました。`Number` 型であることと、（Crystal になくてはならない）`String` 型であるという制約です。

## メソッドのオーバーロード <a id="method-overloading"></a>

これまでたくさんのオーバーロードを見てきました。ここでは `メソッドオーバーロード` について触れます。

メソッドオーバーロードは、同じ名前で違う引数を持っている関数のことを言います。これらはみんな同じ名前を持っていますが、全く異なる関数になります。

`add` 関数をオーバーロードして、String でも動作するようにしましょう。

```ruby
def add(x : Number, y : Number)
  x + y
end

def add(x: Number, y: String)
  x.to_s + y
end

puts add 3, 5

puts add 3, "Serdar"
```

実行してみましょう。

```text
$ crystal types.cr
8
3Serdar
```

これでメソッドオーバーロードが実行できました。Number型とString型をそれぞれ使って、適切な関数を呼んでいることが分かりました。メソッドオーバーロードは好きなだけ定義することができます。