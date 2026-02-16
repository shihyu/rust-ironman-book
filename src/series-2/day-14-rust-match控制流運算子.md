# Rust-Match控制流運算子

- 系列：Rust 新手村
- 來源系列：https://ithelp.ithome.com.tw/2020-12th-ironman/articles/4260
- 原文連結：https://ithelp.ithome.com.tw/articles/10269063
- 章節：DAY 14
- 發佈時間：2021-09-22 08:45:36
- 作者：Ken
- 清單瀏覽數：1115
- 清單摘要：Match是使用枚舉的基本工具，類似Golang的Switch語法 Match取值後對每個條件進行比較依照順序比較，一但匹配成功就對右側求值，並結算Match語...

## 內文

Match是使用枚舉的基本工具，類似Golang的Switch語法

Match取值後對每個條件進行比較依照順序比較，一但匹配成功就對右側求值，並結算Match語法

每個條件分支右側都必須是單個表達式

例

前面三個都是無效的表達式

``` rust
match browser {
    Firefox => let var = 777;, // 無效的表達式
    Chrome => let var = 777, // 無效的表達式
    Ie =>  fn funct() {}, // 無效的表達式
    Safari => println!("S"),
}
```

如果需要在右側好幾個表達式或是非表達式可以用塊來包起來

例

``` rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

let mut browser = Browser::Ie;

match browser {
    Browser::Firefox => {
        browser = Browser::Ie;
        println!("F");
    },
    Browser::Chrome => {
        let var = 777;
        println!("C")
    },
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

處理所有可能

``` rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

let browser = Browser::Ie;

match browser {
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}

// 編譯時會出
patterns `Firefox` and `Chrome` not covered
```

因為少了Firefox和Chrome條件分支，Rust要求match需要顯式的處理所有可能的情況

可以使用空處理

``` rust
let browser = Browser::Ie;

match browser {
    Browser::Firefox => {}, // 使用空處理
    Browser::Chrome => {}, // 使用空處理
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

或是使用"\_"下底線，類似Golang的Default

``` rust
let browser = Browser::Ie;

match browser {
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
    _ => {},
}
```

如果Default放在條件最上面會因為順序關係什麼都不處理

``` rust
let browser = Browser::Ie;

match browser {
    _ => {}, // 永遠都跑到這裡
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

### 對型別使用Match

match除了對枚舉之外也可以對一般型別

``` rust
match "value" {
    "value" => println!("value"),
    _ => println!("other"),
}

match 1 {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("other"),
}
```

因為型別不像枚舉有可窮舉出所有，所以一定需要"\_"(Default)
條件，不然編譯會出現錯誤

## ASCII 詳細示意圖

```text
═══════════════════════════════════════════════════
      match 的全面覆蓋(exhaustive)模型
═══════════════════════════════════════════════════

match value {
  PatternA => arm_a,
  PatternB(x) if guard(x) => arm_b,
  _ => default_arm,
}

─────────────────────────────────────────────────
                     流程
─────────────────────────────────────────────────

┌───────────────────┐
│ input value       │                              ← 取得匹配值
└─────────┬─────────┘
          │
          v
┌───────────────────┐
│ arm 1 pattern?    │──yes──> execute arm 1        ← 依序比對
└─────────┬─────────┘
          │ no
          v
┌───────────────────┐
│ arm 2 pattern?    │──yes──> guard? ──> arm 2 / next
└─────────┬─────────┘                              ← 可附加守衛條件
          │ no
          v
┌───────────────────┐
│ wildcard (_)      │──────> execute default        ← 兜底分支
└───────────────────┘

編譯器保證：所有可能值都要被處理。
```
