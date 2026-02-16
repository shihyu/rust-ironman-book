# Rust-結構體(Struct)

- 系列：Rust 新手村
- 來源系列：https://ithelp.ithome.com.tw/2020-12th-ironman/articles/4260
- 原文連結：https://ithelp.ithome.com.tw/articles/10269136
- 章節：DAY 15
- 發佈時間：2021-09-23 08:46:03
- 作者：Ken
- 清單瀏覽數：1145
- 清單摘要：struct 是命名並封裝數個欄位數值所組合的自訂型別 struct 有 3 種類型 元組結構體(tuple struct)，就是具名元組而已 經典的C語言風...

## 內文

struct 是命名並封裝數個欄位數值所組合的自訂型別

struct 有 3 種類型

- 元組結構體(tuple struct)，就是具名元組而已
- 經典的C語言風格結構體(C struct)
- 單元結構體(unit struct)，不帶字段，在泛型中很有用

前幾篇所講的元組型別(tuple)，只要元組包含少量的字段就很好用但是當字段一多就容易將它們搞混  
並且代碼也不容易理解

例如下面的代碼，不能很輕易的知道哪個字段相加，而且元組型別是有序的如果在開頭增加字段那變成後面的字段都需要往後遞延非常不直覺

``` rust
let data = (1, 'a', 'b', 199.199, true, -100);
println!("{}", data.0 + data.5);
```

這時候使用結構體(Struct)就非常有用，可以為字段命名並標示型別

它以struct關鍵字開頭後面接著要聲明的類型再以大括號包起來

例如

``` rust
// 經典的C語言風格結構體(C struct)
struct User {
    username: String,
    email: String,
    active: bool,
}

let user = User {
    email: String::from("user@usermail.com"),
    username: String::from("user123"),
    active: true,
};

println!("{} {} {}", user.email, user.username, user.active)
```

元組結構，擁有元組型別和結構兩個特性又不完全一樣

- 需要事先聲明
- 字段沒有名稱

實際上這種結構不常使用

``` rust
struct User(String, String, bool);

let user = User(
    String::from("user@usermail.com"),
    String::from("user123"),
    true,
);

println!("{} {} {}", user.0, user.1, user.2)
```

單元結構體

單元結構體適合用在當要實作一個特徵(trait)或某種型別，但沒有任何需要儲存在型別中的資料

``` rust
struct AlwaysEqual;

let subject = AlwaysEqual;
```

## ASCII 詳細示意圖

```text
Struct 欄位佈局與方法呼叫

struct User {
  id: u64,
  active: bool,
  name: String,
}

Stack (User)
+------------------------------------------+
| id (8) | active (1) | padding | name(...)|
+------------------------------------------+
                           |
                           v
                     String header
                     +-------------------------+
                     | ptr | len | cap        |
                     +-------------------------+
                               |
                               v
                            Heap bytes

方法呼叫
user.activate()
== User::activate(&mut user)
```
