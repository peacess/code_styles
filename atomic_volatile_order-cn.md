# 多线程数据安全：原子操作、volatile、执行顺序（cpu/编译）
## 原子操作很好理解，是不可分的
## volatile是可见性，线程有自己的缓存，首先会从缓存中存取变量的值。这样在不同线程中同看到的同一变量的值就可能不同，这就是变量的可见性。它与原子操作没有关系，属于不同的概念。
## 执行顺序。这里有三个相关的
1. 代码顺序，源代码中的先后顺序
2. 编译顺序，经过编译器编译之后的顺序，由于编译器会优化，可能编译顺序与代码顺序是不一样的
3. 执行顺序（一般指的是多线程下的，而不是单线程），cpu在运行时，由于多核或优化或cpu架构，执行顺序可能与编译后的顺序不一样，这也叫乱序执行/错序执行/out-of-order execution    
   Relaxed,没有内存顺序要求，但在同一线程中是有先后的，也就是sequenced-before  
   Release,store,在它之前的保证在它之前  
   Acquire,load,在它之后的保证在它之后  
   AcqRel,是Release或Acquire，使用load用Acquire,store使用Release  
   SeqCst,最严格的，前后都保证  
注：这是rust语言中定义的名字，它与c++中的是一至  
注：写代码时给出最低要求的级别就可以，运行时的级别，很难确定，能确定的是不低于代码给出的级别  
[std::sync::atomic::Ordering](https://doc.rust-lang.org/std/sync/atomic/enum.Ordering.html)  
[c++ memory_order](https://en.cppreference.com/w/cpp/atomic/memory_order)  
[loop,测试Ordering](https://github.com/tokio-rs/loom)  
[Atomics and Memory Ordering](https://riptutorial.com/rust/example/21259/atomics-and-memory-ordering)  
[C++11 atomic x86 memory ordering](https://stackoverflow.com/questions/11836028/c11-atomic-x86-memory-ordering)  
[Rust 并发编程 - Memory Ordering](https://www.jianshu.com/p/511cde6b62a6)  
## 使用说明
### 在多线程下，要表示线程是否退出，使用什么？  
在有volatile关键字的语言中使用volatile就可以了。 而rust使用如下： 
```rust
fn main() {
    let mut status = AtomicBool::new(true);
    //...
    status.load(Ordering::Relaxed);//因为没有顺序要求，这里只要“Ordering::Relaxed”就好了
    //...
    status.store(false, Ordering::Relaxed);
}
```
注：volatile并不能保存是原子的，但对于int32或bool变量在32位的操作系统下面，一次就可以操作32位，所以单独的多线程读写是安全的  
“x = x + 1”是两个操作，它不是原子的，也就是要使用 atomic提供的“+”或使用lock来保证数据安全
### 在多线程下，当有两个int32变量时，使用什么？
如果这两个变量之间有关联，如 "if a==1 then b=1"，就需要特别小心的使用Ordering。例子：
```rust
use std::sync::Arc;
use std::sync::atomic::{AtomicBool, fence, Ordering};
use std::thread;

#[test]
fn main() {
   let a = Arc::new(AtomicBool::new(false));
   let b = Arc::new(AtomicBool::new(false));

   for _ in 0..100 {
      let a_clone = a.clone();
      let b_clone = b.clone();
      let t1 = thread::spawn(move || {
         a_clone.store(true, Ordering::Relaxed);
         fence(Ordering::Release);
         b_clone.store(true, Ordering::Relaxed);
      });

      let a_clone = a.clone();
      let b_clone = b.clone();
      let t2 = thread::spawn(move || {
         while !b_clone.load(Ordering::Relaxed){}
         fence(Ordering::Acquire);
         let a = a_clone.load(Ordering::Relaxed);
         println!("a = {},b = true",a);
      });

      let a_clone = a.clone();
      let b_clone = b.clone();
      let t3 = thread::spawn(move || {
         while !a_clone.load(Ordering::Relaxed){}
         fence(Ordering::Acquire);
         let b = b_clone.load(Ordering::Relaxed);
         println!("a = true,b = {}",b);
      });

      let _ = t1.join();
      let _ = t2.join();
      let _ = t3.join();
   }
}
```
