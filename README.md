# rust_the_book_practice

## Overview

This repository is a for tracking my progress in "The Rust Programming Language 日本語版"

## Progress

2026/01/13 1.1 インストール
2026/01/14 1.2 Hello, world
2026/01/14 1.3 Hello, Cargo

## 事始め

### 1.2 Hello, world

- `main`関数は特別で、常に全ての実行可能なRustプログラムで走る最初のコードになる。
- 関数の引数がある場合`()`の内部に入る
- 関数の本体は`{}`に包まれる。Rustでは、全ての関数本体の周りに波括弧が必要になる。
- 以下のプログラムのようにスペースを一つ開けて、`{`を関数宣言と同じ行に配置するのがいいスタイル

```rust
fn main() {

}
```

- 複数のRustプロジェクトに渡って標準的なスタイルにこだわりたいなら、`rustfmt`をつかうのがいい。[付録D](https://doc.rust-jp.rs/book-ja/appendix-04-useful-development-tools.html)によると、

```zsh
rustup component add rustfmt
```

でインストールでき、その後どんなCargoのプロジェクトも次を入力するとフォーマットできる

```zsh
cargo fmt
```

- main関数の本体は、以下のようなコードを抱えているが、ここでは気づくべきことが4つある。

```rust
    println!("Hello, world!");
```

1. Rustのスタイルはタブではなく、4スペースでインデントするということ。
2. `println!`はRustのマクロを呼び出すといことであるということ。もし仮に関数を呼んでいたら、`println`(!なし)と入力されている。マクロは関数とは同じルールには必ずしも従わない。
3. `"Hello, world!"`という文字列を引数として`println!`に渡しているということ。
4. 式をセミコロンで終わらせていること。rustのほとんどのコードはセミコロンで終わる。

- Rustは**AOTコンパイル言語**である。この利点はプログラムをコンパイルして、実行可能ファイルを誰かにあげ、あげた人がRustをインストールしていなくても実行できるところにある。

### Hello, Cargo

- CargoはRustのビルドシステム兼パッケージマネージャ
- Cargoでプロジェクトを作成する方法は、

```zsh
cargo new hello_cargo
```

とした場合、hello_cargoという名前の新しディレクトリとプロジェクトが作成される。

- Cargo.tomlは依存関係を記録する
- プロジェクトをビルドしたいときは`cargo build`で実行できる。
- `./target/debug/hello_cargo` で実行ファイルを実行できる。
- `cargo run`を使うと、コードのコンパイルから、できた実行ファイルの実行までの全体を一つのコマンドで行える
- `cargo check`コマンドはコードがコンパイルできるか、素早くチェックするが、実行ファイルは生成しない。
