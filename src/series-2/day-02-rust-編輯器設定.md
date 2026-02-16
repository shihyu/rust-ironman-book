# Rust-編輯器設定

- 系列：Rust 新手村
- 來源系列：https://ithelp.ithome.com.tw/2020-12th-ironman/articles/4260
- 原文連結：https://ithelp.ithome.com.tw/articles/10261843
- 章節：DAY 02
- 發佈時間：2021-09-10 08:38:17
- 作者：Ken
- 清單瀏覽數：2853
- 清單摘要：如何設定開發環境這邊使用VSCode來當開發工具 rustfmt 自動格式化 rustfmt是Rust官方提供自動格式化代碼的工具，用來統一代碼風格，避免有人用...

## 內文

如何設定開發環境這邊使用VSCode來當開發工具

### rustfmt 自動格式化

rustfmt是Rust官方提供自動格式化代碼的工具，用來統一代碼風格，避免有人用Tab有人用空格來縮排或是在大括號之後該換行之類的

透過rustup安裝

``` rust
rustup component add rustfmt
```

透過指令來格式化代碼

``` rust
rustfmt main.rs
```

也可以透過cargo格式化整個專案

``` rust
cargo fmt
```

也可以單純檢查並列出沒排好的地方

``` rust
rustfmt --check main.rs

cargo fmt -- --check 
```

預設的rustfmt風格就很好用了，但如果你的團隊或是個人習慣想要不一樣排版風格也是可以的，透過rustfmt.toml設定檔來改變

例如二元運算子多行時要放在頭還是放在尾的部分

``` rust
binop_separator = "Front" (默認)

let or = foofoofoofoofoofoofoofoofoofoofoofoofoofoofoofoo
        || barbarbarbarbarbarbarbarbarbarbarbarbarbarbarbar;

---------------------------------------------------------------
binop_separator = "Back"

let or = foofoofoofoofoofoofoofoofoofoofoofoofoofoofoofoo ||
        barbarbarbarbarbarbarbarbarbarbarbarbarbarbarbar;
```

還有其他很多設定，可以參考這文件

<https://github.com/rust-lang/rustfmt/blob/master/Configurations.md>

讓vscode支援rust代碼提示，需要安裝Rust套件模組

<https://marketplace.visualstudio.com/items?itemName=rust-lang.rust>

安裝時會順便安裝rls, rust-src, and rust-analysis

如果要讓存檔時自動格式化代碼需要修改

File \> Preferences \> Settings. 裡面的 editor.formatOnSave 打勾
就可以了

## ASCII 詳細示意圖

```text
═══════════════════════════════════════════════════════════════════
              編輯器 + Rust 開發回饋循環
═══════════════════════════════════════════════════════════════════

┌──────────────────┐      儲存檔案       ┌───────────────────────┐
│ VSCode / Vim     │────────────────────>│ rust-analyzer (LSP)   │  ← 語言伺服器
│ IntelliJ         │                     │ 型別推導/語法檢查     │
└──────────────────┘                     └───────────┬───────────┘
                                                     │
                    ┌────────────────────────────────┼─────────────────────┐
                    │                                                      │
                    v                                                      v
─────────────────────────────────────────────────────────────────────────────────
         ┌───────────────────────┐                            ┌───────────────────┐
         │ rustfmt               │                            │ cargo check       │
         │ 統一格式              │  ← 自動排版                │ 快速編譯檢查      │
         └───────────────────────┘                            └─────────┬─────────┘
                                                                        │
                                                                        v
                                                              ┌───────────────────┐
                                                              │ clippy            │
                                                              │ lint 與最佳實務   │  ← 程式碼品質
                                                              └───────────────────┘
```
