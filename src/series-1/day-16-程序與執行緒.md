# 程序與執行緒

- 系列：30 天深入淺出 Rust
- 來源系列：https://ithelp.ithome.com.tw/users/20111802/ironman/1742
- 原文連結：https://ithelp.ithome.com.tw/articles/10202133
- 章節：DAY 16
- 發佈時間：2018-10-16 22:41:07
- 作者：DanSnow
- 清單瀏覽數：3260
- 清單摘要：偶爾我們要呼叫外部的程式來幫我們處理一些東西，這個時候就是 std::process 下的東西登場的時候了。 Command Command 可以讓我們呼叫外部...

## 內文

偶爾我們要呼叫外部的程式來幫我們處理一些東西，這個時候就是
[`std::process`](https://doc.rust-lang.org/stable/std/process/index.html)
下的東西登場的時候了。

## Command

`Command` 可以讓我們呼叫外部的程式：

``` rust
use std::process::Command;

let mut cmd = Command::new("ls").spawn().expect("Fail to spawn");
cmd.wait().expect("Fail to wait");
```

這會呼叫 `ls` 列出目前目錄下的檔案，並等待子程序結束。

> 如果你想要像 C 或其它語言的 `system` 一樣，你需要呼叫 shell 。

## 執行緒 (Thread)

現在的電腦大多是多核心的，如果建立執行緒，讓多個執行緒一起處理資料的話理論上就能加快速度了：

``` rust
use std::thread;
use std::time::Duration;

fn main() {
  // 建立執行緒
  let handle = thread::spawn(move || {
    let half_sec = Duration::from_millis(500);
    for _ in 0..10 {
      println!("Thread");
      // 休息半秒
      thread::sleep(half_sec);
    }
  });
  let one_sec = Duration::from_secs(1);
  for _ in 0..5 {
    println!("Main");
    // 休息 1 秒
    thread::sleep(one_sec);
  }
  // 等待子執行緒結束
  handle.join().unwrap();
}
```

這個與接下來的範例都請在自己的電腦上執行，這邊你應該會看到它們兩個以不同的速度在輸出。

## Arc 與 Mutex

還記得之前介紹 `Rc` 時提過 `Arc` 嗎？ `Rc` 與 `Arc` 很像，只是 `Arc`
能跨執行緒的共享資料，在使用多執行緒時我們必須要使用 `Arc` ，而 `Mutex`
是什麼？ `Mutex`
中文為互斥鎖，功能是保護資料不會因為多個執行緒修改而發生資料競爭 (data
racing)
或是其它競爭情況，這邊並不詳細解釋，有興趣可以看一下[維基](https://en.wikipedia.org/wiki/Race_condition#Example)的說明，
Rust 中跟其它語言不同，一個有趣的地方是 `Mutex`
也是個容器，讓你直接包裝需要保護的資料：

``` rust
use std::thread;
use std::sync::{Arc, Mutex};
use std::time::Duration;

fn main() {
  // 這邊使用 Arc 來包裝 Mutex 這樣才能在執行緒間共享同一個 Mutex
  let data = Arc::new(Mutex::new(0));
  let mut children = Vec::new();
  let one_sec = Duration::from_secs(1);
  // 開 4 個執行緒，因為我的電腦是 4 核的，你可以自己決定要不要修改
  // 另外你也可以把數字改成 1 ，這樣就跟沒有平行化是一樣的，你可以比較一下速度的差別
  for i in 0..4 {
    // 使用 clone 來共享 Arc
    let data = data.clone();
    children.push(thread::spawn(move || loop {
      {
        // 鎖定資料
        let mut guard = data.lock().unwrap();
        // 如果大於 10 就結束
        if *guard >= 10 {
          println!("Thread[{}] exit", i);
          break;
        } else {
          // 處理資料
          *guard += 1;
        }
        println!("Thread[{}] data: {}", i, *guard);
        // 離開 scope 時會自動解鎖 Mutex
      }
      // 模擬處理的耗時
      thread::sleep(one_sec);
    }));
  }
  // 等所有執行緒結束
  for child in children {
    child.join().unwrap();
  }
}
```

> 一般都會盡可能減少鎖定資料的範圍，這樣才能盡量的平行化。

## rayon

[rayon](https://github.com/rayon-rs/rayon) 是個很方便的平行化的 crate
，它可以讓原本迭代器做的事情變成平行化處理，而且使用非常簡單：

``` rust
extern crate rayon;

// 引入必要的東西
use rayon::prelude::*;

fn main() {
    let mut data = Vec::new();
    for i in 1..=1000000 {
        data.push(i);
    }
    // 只要在原本的 iter 前加上 par_ 就行了
    data.par_iter_mut().for_each(|x| *x *= 2);
}
```

下一篇要再來介紹更多跟執行緒與平行處理有關的東西。

## ASCII 詳細示意圖

```text
═══════════════════════════════════════
  程序(Process) 與執行緒(Thread)
═══════════════════════════════════════

Process A
┌─────────────────────────────────┐
│  記憶體空間 (獨立)              │
│                                 │
│  ├── Thread-1 (stack1)          │  ← 各自獨立的堆疊
│  ├── Thread-2 (stack2)          │  ← 各自獨立的堆疊
│  └── Shared heap                │  ← 所有執行緒共享
└─────────────────────────────────┘

───────────────────────────────────────
  通訊方式
───────────────────────────────────────

不同 Process 間需 IPC
同 Process Thread 可共享 heap
```
