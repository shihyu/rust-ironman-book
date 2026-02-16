# Rust-定義函式Function(一)

- 系列：Rust 新手村
- 來源系列：https://ithelp.ithome.com.tw/2020-12th-ironman/articles/4260
- 原文連結：https://ithelp.ithome.com.tw/articles/10269169
- 章節：DAY 16
- 發佈時間：2021-09-24 10:22:06
- 作者：Ken
- 清單瀏覽數：1281
- 清單摘要：如果編寫多次相同的代碼，則可以把代碼封裝在一個塊中，然後為該代碼命名 通過這種方式就定義了函式，然後可以通過命名的名稱來調用該函式 要訂一個函式需要使用&quo...

## 內文

如果編寫多次相同的代碼，則可以把代碼封裝在一個塊中，然後為該代碼命名

通過這種方式就定義了函式，然後可以通過命名的名稱來調用該函式

要訂一個函式需要使用"fn"關鍵字後面接著函式的名稱跟圓括號然後是一個大括號塊

大括號的塊稱為函式體，函式體之前都稱為簽名

下面例子簡單使用函式

``` rust
fn hello_word() {
    println!("hello world!")
}

hello_word();
hello_word();
hello_word();

輸出
hello world!
hello world!
hello world!
```

後定義函式

``` rust
a;
let a = 5; // 非法使用變數

hello_word(); // 合法使用後定義的函式
fn hello_word() {
    println!("hello world!")
}
```

函式屏蔽其他函式

``` rust
fn hello_word() {}
fn hello_word() {} // 多次定義函式

編譯時會出錯
the name `hello_word` is defined multiple times
```

但是可以包在塊裡多次定義fn

``` rust
{
    fn hello_word() {
        println!("hello world 1")
    }

    hello_word();
}
{
    fn hello_word() {
        println!("hello world 2")
    }

    hello_word();
}

輸出
hello world 1
hello world 2
```

每個定義函式只能在塊裡面有效，下面是不合法的

``` rust
{
    fn hello_word() {
        println!("hello world 2")
    }
}

hello_word();

編譯時會出錯
cannot find function `hello_word` in this scope
```

也可以在屏蔽外的塊級定義另一個函式

在main外部定義函式hello_word()，因其內部也定義了hello_word，所以永遠用不到外部的定義函式

通常編譯器會警告

``` rust
fn hello_word() {
    println!("hello world 1")
}

fn main() {
    hello_word();

    {
        hello_word();
        fn hello_word() {
            println!("hello world 2")
        }
    }

    hello_word();
    fn hello_word() {
        println!("hello world 3")
    }
}

這邊輸出
hello world 3
hello world 2
hello world 3
```

## ASCII 詳細示意圖

```text
函式定義與呼叫堆疊

main()
  |
  +--> add(2, 3)
          |
          v
   +----------------------+
   | Stack Frame: add     |
   | a = 2                |
   | b = 3                |
   | return slot          |
   +----------+-----------+
              |
              v
         return 5
              |
              v
   +----------------------+
   | Stack Frame: main    |
   | result = 5           |
   +----------------------+

參數與回傳型別皆需明確，提升可讀性與型別安全。
```
