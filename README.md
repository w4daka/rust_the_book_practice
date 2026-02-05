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

### 変数と可変性

```rust
fn main() {
    let x = 10;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

といううコードを実行しようとすると、以下のようなエラーが発生する。

```shell
   Compiling variables v0.1.0 (/home/doxaripo/projects/rust_
the_book_practice/variables)
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 10;
  |         - first assignment to `x`
3 |     println!("The value of x is: {}", x);
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable
  |
help: consider making this binding mutable
  |
2 |     let mut x = 10;
  |         +++

For more information about this error, try `rustc --explain
E0384`.
error: could not compile `variables` (bin "variables") due t
o 1 previous error

[Process exited 101]
```

これの説明は以下の通り。

---

An immutable variable was reassigned.

Erroneous code example:

```rust
fn main() {
    let x = 3;
    x = 5; // error, reassignment of immutable variable
}
```

By default, variables in Rust are immutable. To fix this error, add the keyword
`mut` after the keyword `let` when declaring the variable. For example:

```
fn main() {
    let mut x = 3;
    x = 5;
}
```

Alternatively, you might consider initializing a new variable: either with a new
bound name or (by [shadowing]) with the bound name of your existing variable.
For example:

[shadowing]: https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#shadowing

```
fn main() {
    let x = 3;
    let x = 5;
}
```

---

和訳(自分)

---

---
