# 第2章: Crystal のインストール

### バイナリインストーラ <a id="binary-installers"></a>

Crystal プロジェクトは公式のバイナリインストーラを配布しています。リリース版、Nightly版双方を入手可能です。バイナリインストーラがもっとも早く手軽にCrystalを使う方法です。Crystal は Crystal そのもので記述されているため、Crystal コンパイラをコンパイルするには、3回コンパイルする必要があるのです。これはかなり面倒ですが、バイナリインストーラなら手軽です。

Crystal には [インストールガイド](http://crystal-lang.org/docs/installation/index.html) があります。一読して、適切なバージョンを入手してください。

本書は Crystal 0.28.0 をもとに記述しています。Nightly版を使う際には、違いがあるかもしれません。

### ソースからインストール <a id="from-source"></a>

もしソースからビルドしたい場合は、Nightly版を使うとよいでしょう。コードサンプルのバグが治っています。

ソースからビルドする際は [Crystal README](http://crystal-lang.org/docs/installation/from_source_repository.html) の手順にしたがってビルドしてください。

#### 将来の変更について <a id="future-proofing"></a>

本書は 0.28.0 をもとに記述しています。言語自体は安定していますが、標準ライブラリや主要なサブシステムは修正途中です。新しいバージョンがリリースされた際は本書を更新していきたいと考えています。

Crystal を引数なしで実行すると、ヘルプが出力されます。参考にしてください。

```text
$ crystal
```
