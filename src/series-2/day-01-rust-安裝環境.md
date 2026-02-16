# Rust-安裝環境

- 系列：Rust 新手村
- 來源系列：https://ithelp.ithome.com.tw/2020-12th-ironman/articles/4260
- 原文連結：https://ithelp.ithome.com.tw/articles/10261723
- 章節：DAY 01
- 發佈時間：2021-09-09 08:48:42
- 作者：Ken
- 清單瀏覽數：1898
- 清單摘要：安裝rustup rustup是rust版本管理器 安裝指令如下 curl --proto '=https' --tlsv1.2 -sSf https://sh...

## 內文

### 安裝rustup

rustup是rust版本管理器

安裝指令如下

``` jsx
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

接著安裝rust總共有三個版可以選擇

- stable
- beta
- nightly

``` bash
rustup install stable
```

輸入下面指令檢查版本，如果有顯示就是代表安裝成功

``` bash
rustc --version
rustc 1.54.0 (a178d0322 2021-07-26)
```

也可以透過這個 ~/.cargo/bin 目錄查看到安裝的rust工具

``` bash
ls ~/.cargo/bin

cargo cargo-clippy cargo-fmt cargo-miri clippy-driver rls rust-gdb rust-lldb rustc rustdoc rustfmt rustup
```

### 升級rust

``` bash
rustup update
```

### 切換rust版本

查看目前所有安裝的版本

``` bash
ls ~/.rustup/toolchains/

beta-x86_64-apple-darwin
stable-x86_64-apple-darwin
```

切換預設版本

``` bash
rustup default beta-x86_64-apple-darwin

info: using existing install for 'beta-x86_64-apple-darwin'
info: default toolchain set to 'beta-x86_64-apple-darwin'

  beta-x86_64-apple-darwin unchanged - rustc 1.55.0-beta.9 (27e88d367 2021-08-28)
```

針對專案設定版本

``` bash
rustup override set nightly

info: using existing install for 'beta-x86_64-apple-darwin'
info: override toolchain for '/Users/ken_jan/rust/hello' set to 'beta-x86_64-apple-darwin'

  beta-x86_64-apple-darwin unchanged - rustc 1.55.0-beta.9 (27e88d367 2021-08-28)
```

### 卸載rust

``` bash
rustup self uninstall
```

如果你只是想嘗試看看，完成不想安裝，也可以使用瀏覽線上版本

[Rust Playground](https://play.rust-lang.org/)

相對於golang的安裝，rust官方就內建版本控制方便許多

這部分rust大勝

## ASCII 詳細示意圖

```text
═══════════════════════════════════════════════════
          Rust 安裝與工具鏈關係圖
═══════════════════════════════════════════════════

┌──────────────────┐       ┌──────────────────────────────┐
│ 使用者 Shell     │──────>│ rustup-init / rustup update  │  ← 安裝/更新入口
└──────────────────┘       └──────────────┬───────────────┘
                                          │
                                          v
                           ┌──────────────────────────────┐
                           │ ~/.cargo/bin                  │  ← 工具安裝目錄
                           │  ├─ rustup                    │
                           │  ├─ rustc                     │
                           │  └─ cargo                     │
                           └──────────────┬───────────────┘
                                          │
─────────────────────────────────────────────────────────────
                                          │
                                          v
                    ┌─────────────────────────────────────────┐
                    │ rustup toolchain                        │  ← 版本管理
                    │ stable (預設) / beta / nightly          │
                    └────────────────────┬────────────────────┘
                                         │
                                         v
                  ┌───────────────────────────────────────────────┐
                  │ cargo new ──> cargo build ──> cargo run/test  │  ← 開發流程
                  └───────────────────────────────────────────────┘
```
