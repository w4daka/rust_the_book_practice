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
不変の変数が再割り当てされた。
誤ったコードの例　
```rust
fn main() {
    let x = 3;
    x = 5; // error, reassignment of immutable variable
}
```
デフォルトでは、Rustの変数は、不変である。このエラーを修正するためには変数を宣言する際に、`mut`キーワードを`let`キーワードの後に加えることが必要。
例
```rust
fn main() {
    let mut x = 3;
    x = 5;
}
```
あるいは、新しい変数に初期化することをあなたは考えるかもしれない。それは、新しい束縛名を用いるか、シャドーイングとして、既存の変数の束縛名を用いるかのいずれかである。
例
```rust
fn main() {
    let x = 3;
    let x = 5;
}
```
---
### Geminiによるシャドーイング解説
⚡️ Rustシャドーイングの総まとめ
1. 概念の定義
 * シャドーイング: 同じスコープ内で、以前に定義した変数と同じ名前で新しい変数を宣言すること。
 * 仕組み: let キーワードを再利用することで、古い変数は「隠され（shadowed）」、以降その名前は新しい変数を指すようになります。

メリット1: 型の変換→文字列から数値への変換など、論理的に同じ対象のデータ型をスムーズに変えられる。 

メリット2: 不変性の維持→mut（可変）を使わずに値を「更新」したような状態を作れるため、意図しない書き換えを防げる。

メリット3: コードの清掃→x_str, x_int のような一時的な変数名を量産せず、名前空間を綺麗に保てる。 

デメリット1:隠蔽のリスク→元の値をまだ使いたかった場合に、アクセスできなくなる。

デメリット2: 可読性の低下→使いすぎると、今その変数がどの型でどんな状態なのか、一目で判別しにくくなる。

3. メモリとスコープの挙動
シャドーイングが行われると、メモリ上では以下のようなことが起きています。
 * 新しい領域の確保: 古い変数（A）を書き換えるのではなく、新しい変数（B）がスタック上に作られます。
 * 名前の紐付け変更: プログラムがその名前を呼んだとき、コンパイラは「より新しい方」を参照します。
 * スコープ終了時の復元: 内側のブロック { } でシャドーイングした場合、そのブロックを抜けると「隠されていた古い変数」が再び表舞台に戻ります。
4. mut（可変変数）との違い
ここが最も混同しやすいポイントです。
 * let mut x = ...: 同じ「箱」の中身を詰め替えるイメージ。型は変えられません。
 * Shadowing (let x = ...): 新しい「箱」を目の前に置くイメージ。全く別の型を入れることができます。
まとめの一言
Rustにおいてシャドーイングは、**「データの変遷（変換）を、安全かつスマートに記述するためのツール」**です。
> 使い分けのコツ:
> 「中身だけをこまめに更新したい」なら mut。
> 「データの形式を変換して、古い形式はもう忘れたい」なら シャドーイング。
> 
シャドーイングについて、他に気になる挙動（例えば所有権との関係など）はありますか？より深い解説が必要であれば、いつでもお伝えください。

