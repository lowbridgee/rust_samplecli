# 実践Rustプログラミング入門写経

なんか書く

## 4章

### deriveマクロ動かん

```
[lowbridgee@03/26 23:29:35] cargo run                                                                                                                                                                                        (git)-[master]
   Compiling samplecli v0.1.0 (/home/lowbridgee/samplecli)
error: cannot find derive macro `Parser` in this scope
 --> src/main.rs:3:10
  |
3 | #[derive(Parser, Debug)]
  |          ^^^^^^
  |
note: `Parser` is imported here, but it is only a trait, without a derive macro
 --> src/main.rs:1:5
  |
1 | use clap::Parser;
  |     ^^^^^^^^^^^^

error: cannot find attribute `clap` in this scope
 --> src/main.rs:4:3
  |
4 | #[clap(
  |   ^^^^
  |
  = note: `clap` is in scope, but it is a crate, not an attribute

e
```

バージョンが紙面と違うが同じように死んだので上げてみたが治らなかった
featuresでderiveを入れると治る、公式に書いてあったわ
https://crates.io/crates/clap

```Cargo.toml
[dependencies]
clap = { version = "3.1.6", features = ["derive"] }
```