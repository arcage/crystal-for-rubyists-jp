# 第2章: Crystal のインストール

### バイナリインストーラ <a id="binary-installers"></a>

Crystal プロジェクトは公式のバイナリインストーラを配布しています。リリース版、Nightly版双方を入手可能です。バイナリインストーラがもっとも早く手軽にCrystalを使う方法です。なぜなら Crystal コンパイラは Crystal 言語自身で記述されているため、Crystal コンパイラをソースからビルドするには、都合3回コンパイルする必要があり非常に面倒だからです。一方、バイナリインストーラならお手軽です。

Crystal には [インストールガイド](http://crystal-lang.org/docs/installation/index.html) があるので、ぜひ一読して、適切なバージョンを入手してください。

注：本書は Crystal 0.28.0 をもとに書かれていいるため、最新のNightly版ではいくつかの変更が加わっている可能性があります。

### ソースからインストール <a id="from-source"></a>

Crystal コンパイラをソースからビルドしたいなどでNightly版を使用するのであれば、サンプルコードのバグに備えてください。重ねて言いますが、この本はCrystal 0.28.0 用に書かれています。

ソースからビルドするのであれば、 [Crystal README](http://crystal-lang.org/docs/installation/from_source_repository.html) が非常に役に立ちます。基本的にこの手順にしたがってビルドしてください。

#### 将来の変更について <a id="future-proofing"></a>

本書は 0.28.0 をもとに記述しています。言語自体はそれなりに安定していますが、標準ライブラリや主要なサブシステムは修正途上です。新しいバージョンがリリースされた際は本書を更新していきたいと考えています。

crystal コマンドを引数なしで実行してヘルプが出力されれば、Crystal を使う準備は万端です。

```text
$ crystal
```

