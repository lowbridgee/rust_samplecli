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

### unused指摘してくれるのはえらあじ

```
[lowbridgee@03/27 19:21:31] cargo run -- input.txt                                                        (git)-[main]
warning: unused variable: `verbose`
  --> src/main.rs:31:33
   |
31 | fn run(reader: BufReader<File>, verbose: bool) {
   |                                 ^^^^^^^ help: if this is intentional, prefix it with an underscore: `_verbose`
   |
   = note: `#[warn(unused_variables)]` on by default

warning: `samplecli` (bin "samplecli") generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/samplecli input.txt`
1 1 +
1 2 + 3 4 + *
1000 1000 *
```

### テスト前までおわり

```
[lowbridgee@03/27 20:53:55] cargo run -- -v                                                               (git)-[main]
   Compiling samplecli v0.1.0 (/home/lowbridgee/samplecli)
    Finished dev [unoptimized + debuginfo] target(s) in 0.85s
     Running `target/debug/samplecli -v`
1 1 1 + +
["+", "+", "1", "1"] [1]
["+", "+", "1"] [1, 1]
["+", "+"] [1, 1, 1]
["+"] [1, 2]
[] [3]
3
^C
[lowbridgee@03/27 21:14:06] cargo run -- input.txt                                                        (git)-[main]
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/samplecli input.txt`
2
21
1000000
[lowbridgee@03/27 21:14:16] cat input.txt                                                                 (git)-[main]
1 1 +
1 2 + 3 4 + *
1000 1000 *
[lowbridgee@03/27 21:15:08]
```