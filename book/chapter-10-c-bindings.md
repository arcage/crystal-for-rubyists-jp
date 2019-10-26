# 第10章: C バインディング

世の中には、C言語で作られた便利なライブラリが数多く存在します。なにかを一から作り直すより、ありものを使うことが重要です。

Crystal では、バインディングをもちいてとても簡単に既存の C ライブラリを使うことができます。

たとえば Clystal では `Regex` クラスの実装として `libpcre` を使っています。

C のバインディングは先ほども触れたとおり、とても簡単に書けます。

```ruby
@[Link("pcre")]
lib LibPCRE
...
end
```

たった3行で `libpcre` をリンクできました。`lib` キーワードを使って、そのライブラリに所属する関数や型をひとまとめにします。ライブラリの宣言のプレフィックスを `Lib` で始めるとよいでしょう。

次は、C の関数を `fun` キーワードでバインドしてみましょう。

```ruby
@[Link("pcre")]
lib LibPCRE
  type Pcre = Void*
  fun compile = pcre_compile(pattern : UInt8*, options : Int, errptr : UInt8**, erroffset : Int*, tableptr : Void*) : Pcre
end
```

上記では `libpcre` の関数を適切な型にバインドしています。これで、簡単に Crystal のコードからこの関数にアクセスできます。

```ruby
LibPCRE.compile(..)
```
