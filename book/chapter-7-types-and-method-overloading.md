# Chapter 7: 型と関数オーバーロード

Crystal is like Ruby, but it’s not Ruby!

Crystal は Ruby と似ていますが、Ruby とは違います。

Unlike Ruby, Crystal is a statically typed and compiled language. Most of the time you don’t have to specify the types and the compiler is smart enough to do `type inference`.

Ruby とは違って、Crystal は静的型付け言語で、コンパイル型言語です。ほとんどの場合、型を宣言する必要はなく、コンパイラがスマートに `型推論` をしてくれます。

So why do we need types? Let’s start with something simple.

型が必要なケースを、簡単な例で見てみましょう。

```ruby
def add(x, y)
  x + y
end

add 3, 5 # 8
```

This is the same in Ruby! We just defined a method that adds two numbers. What if we try to add a number to a string?

これは Ruby と同じです。ふたつの数字を足し合わせるメソッドを定義しました。これで、数字と文字列を足し合わせるとどうなるでしょう？

```ruby
add 3, "Serdar"
```

First let’s do that in Ruby.

まず、Ruby で実行してみましょう。

```text
types.cr:2:in `+': String can't be coerced into Fixnum (TypeError)
    from types.cr:2:in `add'
    from types.cr:5:in `<main>'
```

What??? We just got a `TypeError` but we don’t have to care about types in Ruby \( or not :\)\). This is also a `runtime error` meaning that your program just crashed at runtime \(definitely not good\).

`TypeError` が返りましたが、Ruby では型を考慮する必要はなかったはずです。これは `実行時エラー` とも呼ばれ、プログラムが実行時にクラッシュしたことを意味します。

Now let’s do the same in Crystal.

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

Okay, that’s quite a scary output but actually it’s great. Our Crystal code didn’t compile and also told us that there’s no overload for `Int32#+` and showed us the possible overloads. This is a `compile time error` meaning that our code didn’t compile and we catch the error before running the program. Lovely!

出力にはだいぶビビりますが、Crystal ではコードはコンパイルされず、「`Int32#+` をオーバーロードする関数が無い」こと、代わりとなりそうなオーバーロードを教えてくれます。これは `コンパイル時エラー` であり、プログラムを実行する前にエラーを検知してくれたことを示しています。

Now let’s add some types and restrict that method to only accept `Number`s.

次に、型を追加してｋメソッドが `Number` 型だけを受け取るように制限してみましょう。

```ruby
def add(x : Number, y : Number)
  x + y
end

puts add 3, "Serdar"
```

Run it.

実行します。

```ruby
Error in ./types.cr:5: no overload matches 'add' with types Int32, String
Overloads are:
- add(x : Number, y : Number)

puts add 3, "Serdar"
     ^~~
```

Awesome! Our program didn’t compile again. And this time with shorter and more accurate error output. We just used `type restriction` on `x` and `y`. We restricted them to be `Number` and Crystal is smart enough to stop us from using the method with a `String`.

## Method Overloading  <a id="method-overloading"></a>

We just saw a lot of overloads. Let’s talk about `Method Overloading`.

Method overloading is having different methods with the same name and different number of arguments. They all have the same name but actually they are all different methods.

Let’s overload our `add` method and make it work with a String.

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

Let’s run it.

```text
$ crystal types.cr
8
3Serdar
```

Now, that’s method overloading in action. It figured out that we are calling the method with a Number and String and called the appropriate method. You can define as many overload methods as you wish.

