# Rust-Hello, World!

- 系列：Rust 新手村
- 來源系列：https://ithelp.ithome.com.tw/2020-12th-ironman/articles/4260
- 原文連結：https://ithelp.ithome.com.tw/articles/10262411
- 章節：DAY 04
- 發佈時間：2021-09-11 11:30:10
- 作者：Ken
- 清單瀏覽數：1512
- 清單摘要：先建立一個hello的目錄，編輯main.rs fn main() { print!("Hello, World!"); } 儲存...

## 內文

先建立一個hello的目錄，編輯main.rs

``` rust
fn main() {
    print!("Hello, World!");
}
```

儲存然後在Terminal用rustc編譯

``` rust
rustc main.rs
```

會產生出main檔案，執行main就會出現Hello, world!了

``` bash
./main
Hello, World!
```

### 分析程式碼

- **fn**是rust的關鍵字，function的簡寫
- main是函式名稱，執行rust程式時將執行，如果沒有main那可能是一個library
- print! 是rust的巨集(macro) 如果沒有"!"表示是函式有的話則是巨集
- "Hello, world!" 表示字串內容
- 最後用";"表示語法結束

rust對大小寫是敏感的

### Print! 用法

利用"{}"佔位符輸出字串，類似golang的fmt.Printf("%s, %s!", "Hello",
"World")

``` rust
print!("{}, {}!", "Hello", "World")

輸出
Hello, World!
```

利用"{}"佔位符輸出數字，類似golang的fmt.Printf("%s: %d", "Num", 9527)

``` rust
print!("{}: {}", "Num", 9527)

輸出
Num: 9527
```

用"\n"輸出多行字串

``` rust
print!("Hello, World!\nHello, World!\nHello, World!")

輸出
Hello, World!
Hello, World!
Hello, World!
```

## ASCII 詳細示意圖

```text
Hello World 從原始碼到輸出的流程

main.rs
  |
  v
+---------------------------+
| rustc / cargo build       |
| 1. 詞法/語法解析          |
| 2. MIR/LLVM IR            |
| 3. 產生執行檔             |
+-------------+-------------+
              |
              v
+---------------------------+
| 可執行檔                  |
| main() -> println!(...)   |
+-------------+-------------+
              |
              v
+---------------------------+
| stdout                    |
| Hello, world!             |
+---------------------------+

println! 是 macro: 先展開，再編譯。
```
