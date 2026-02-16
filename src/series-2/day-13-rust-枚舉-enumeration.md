# Rust-枚舉(enumeration)

- 系列：Rust 新手村
- 來源系列：https://ithelp.ithome.com.tw/2020-12th-ironman/articles/4260
- 原文連結：https://ithelp.ithome.com.tw/articles/10268997
- 章節：DAY 13
- 發佈時間：2021-09-21 13:08:58
- 作者：Ken
- 清單瀏覽數：870
- 清單摘要：枚舉就是列出有窮序列的型別 透過enum關鍵字新增了新的Browser型別在範例中列出了一個組項分別為 Firefox，Chrome，IE，Safari內部值分...

## 內文

枚舉就是列出有窮序列的型別

透過enum關鍵字新增了新的Browser型別在範例中列出了一個組項分別為

Firefox，Chrome，IE，Safari內部值分別為0u8，1u8，2u8，3u8表示

通常都是以整數為內部關聯值

透過枚舉寫以下的代碼

``` rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

let browser = Browser::Ie;

match browser {
    Browser::Firefox => println!("F"),
    Browser::Chrome => println!("C"),
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

不要編寫這樣的代碼

``` rust
const FIREFOX: u8 = 0;
const CHROME: u8 = 1;
const IE: u8 = 2;
const SAFARI: u8 = 3;

let browser = IE;

if browser == FIREFOX {
    println!("F");
} else if browser == CHROME {
    println!("C");
} else if browser == IE {
    println!("I");
} else if browser == SAFARI {
    println!("S");
}
```

### 再怎樣也不要寫[magic number](https://zh.wikipedia.org/wiki/%E9%AD%94%E8%A1%93%E6%95%B8%E5%AD%97_(%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88))

### 如何使用枚舉

使用Use的方式

``` rust
// 顯式的指定要使用的枚舉
use Browser::{Chrome, Firefox, Ie, Safari};

let browser = Ie;

match browser {
    Firefox => println!("F"),
    Chrome => println!("C"),
    Ie => println!("I"),
    Safari => println!("S"),
}

// 自動的使用Browser內部所有的枚舉
use Browser::*;

let browser = Ie;

match browser {
    Firefox => println!("F"),
    Chrome => println!("C"),
    Ie => println!("I"),
    Safari => println!("S"),
}
```

使用帶有C風格的用法

``` rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

println!("{}", Browser::Firefox as i32)
```

枚舉不能使用"=="運算子做比較

``` rust
let browser = Browser::Ie;

if browser == Browser::Ie {
    println!("{}", "hello world")
}

// 編譯時會出錯
// binary operation `==` cannot be applied to type `Browser`
```

## ASCII 詳細示意圖

```text
Enum 的判別值(discriminant) + 載荷(payload)

enum Message {
  Quit,
  Move { x: i32, y: i32 },
  Write(String),
}

記憶體概念
+----------------------------------------------+
| discriminant | payload union                 |
+----------------------------------------------+
| Quit         | (empty)                       |
| Move         | x: i32, y: i32                |
| Write        | String { ptr, len, cap }      |
+----------------------------------------------+

Option<T> 常見模型
Some(T) / None
+----------------------+ 
| tag + optional value |
+----------------------+

優點：用型別取代 magic number 與不安全狀態碼。
```
