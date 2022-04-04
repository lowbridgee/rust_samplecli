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

### テスト編

環境に応じたコンパイルについて
https://doc.rust-jp.rs/rust-by-example-ja/attribute/cfg.html

除算の実装抜けとる

```
[lowbridgee@03/27 21:20:44] cargo test                                                                    (git)-[main]
   Compiling samplecli v0.1.0 (/home/lowbridgee/samplecli)
    Finished test [unoptimized + debuginfo] target(s) in 0.72s
     Running unittests (target/debug/deps/samplecli-2a66fd288a395caa)

running 1 test
test tests::test_ok ... FAILED

failures:

---- tests::test_ok stdout ----
thread 'tests::test_ok' panicked at 'invalid token', src/main.rs:69:26
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    tests::test_ok

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

error: test failed, to rerun pass '--bin samplecli'
[lowbridgee@03/27 21:30:11]
```

### エラーハンドリング編

エラー処理クレート前まで

```
[lowbridgee@03/27 21:55:50] echo 42 > number.txt                                                          (git)-[main]
[lowbridgee@03/27 23:56:46] cargo run --bin err_panic                                                     (git)-[main]
   Compiling samplecli v0.1.0 (/home/lowbridgee/samplecli)
    Finished dev [unoptimized + debuginfo] target(s) in 0.92s
     Running `target/debug/err_panic`
84
[lowbridgee@03/27 23:56:56] echo hoge > number.txt                                                        (git)-[main]
[lowbridgee@03/28 00:05:18] cargo run --bin err_panic                                                     (git)-[main]
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/err_panic`
thread 'main' panicked at 'failed to parse string to a number: ParseIntError { kind: InvalidDigit }', src/bin/err_panic.rs:9:10
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
[lowbridgee@03/28 00:05:21]
```

## 完成

```
[lowbridgee@04/05 00:39:02] cargo install --path .                                                        (git)-[main]
  Installing rpncalc v0.1.0 (/home/lowbridgee/samplecli)
    Updating crates.io index
  Downloaded clap_derive v3.1.7
  Downloaded clap v3.1.8
  Downloaded syn v1.0.90
  Downloaded indexmap v1.8.1
  Downloaded 4 crates (521.0 KB) in 0.75s
   Compiling proc-macro2 v1.0.36
   Compiling unicode-xid v0.2.2
   Compiling version_check v0.9.4
   Compiling syn v1.0.90
   Compiling libc v0.2.121
   Compiling memchr v2.4.1
   Compiling autocfg v1.1.0
   Compiling hashbrown v0.11.2
   Compiling anyhow v1.0.56
   Compiling heck v0.4.0
   Compiling bitflags v1.3.2
   Compiling lazy_static v1.4.0
   Compiling strsim v0.10.0
   Compiling termcolor v1.1.3
   Compiling textwrap v0.15.0
   Compiling proc-macro-error-attr v1.0.4
   Compiling proc-macro-error v1.0.4
   Compiling indexmap v1.8.1
   Compiling os_str_bytes v6.0.0
   Compiling quote v1.0.17
   Compiling atty v0.2.14
   Compiling thiserror-impl v1.0.30
   Compiling clap_derive v3.1.7
   Compiling thiserror v1.0.30
   Compiling clap v3.1.8
   Compiling rpncalc v0.1.0 (/home/lowbridgee/samplecli)
    Finished release [optimized] target(s) in 15.29s
  Installing /home/lowbridgee/.cargo/bin/err_anyhow
  Installing /home/lowbridgee/.cargo/bin/err_panic
  Installing /home/lowbridgee/.cargo/bin/err_thiserror
  Installing /home/lowbridgee/.cargo/bin/rpncalc
   Installed package `rpncalc v0.1.0 (/home/lowbridgee/samplecli)` (executables `err_anyhow`, `err_panic`, `err_thiserror`, `rpncalc`)
[lowbridgee@04/05 00:40:19] rpncalc                                                                       (git)-[main]
1 1 +
2
1 2 + 3 4 + *
21
+ 1 1
invalid syntax at 1
```