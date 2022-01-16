[中文](./codestyle_rust-cn.md)

# Code Style -- rust 0.3

## 名词

函数(function)：由fn定义的函数  
方法(method)：是一种特殊的函数，第一个参数含self（与一个struct或trait关联的）  
关联函数(Associated functions)：Associated functions are functions associated with a type  
孤儿原则：trait与实现trait，有这样的要求。 impl的代码要么在trait所在的crate，要么在struct所在的crate。
FnX：是Fn,FnMut,FnOnce中的一种或多种

## 规则
[参见](./development_roles-cn.md)

```rust
fn fun_name() -> Vec<i32>{
   //定义数据
   let mut data = Vec::new();
   //生成数据
   {}
   //使用数据
   return data;
}
```

## Name

[遵守Rust的命名](https://rust-lang.github.io/api-guidelines/naming.html)  
[RFC0430 finalizing naming conventions](https://github.com/rust-lang/rfcs/blob/master/text/0430-finalizing-naming-conventions.md)  
常用单词[Words](./words_cn_en.md)   

1. 所有源代码文件名，使用小写，加下划线
2. toml中的package或bin 命名使用下划线，不使用减号， 保持package name 与crate或lib 的名字一至
2. 所有目录文件名，使用小写，加下划线
3. 命名使用有明确函义的英文单词
4. 不要翻译lifetime这个单词，因为它的通常翻译是“生命周期”，这个不能很准备的表达英文的意思，“生命周期”更适合与lifecycle对应
5. 优先选用as_与into_打头的方法，然后再是to_打头的方法。  
    std在实现时以as_/into_打头的方法，一般是没有代价的。而to_一般会做较多工作如内存分配等，如方法str::to_lowercase()转换为小写，一般会新分配内存。  
    str::as_bytes查看str的uft-8的字节，没有内存分配。String::into_bytes进入内部的Vec<u8>数据。  
    as 与 into很像， as转换类型查看，into是进入里面的类型查看。查看源代码String::to_bytes/into_bytes体会它们的区别
6. iter/iter_mut/into_iter，其中into_iter是生成独立的iter，它会消耗容器，也就是owened。
7. 使用一至的单词顺命名。  
    一般是动词-名词-错误。这里有一种特殊的情况是，是某一分类下内容，可以在动词前面增加一个分类的前缀如 Eth---，或Btc---等

## 目录文件

1. 单元测试？
2. 所有的demo放入“ 仓库名/demo ” 目录中
3. 如果是框架或基础库，需要“仓库名/sample”

## 代码

1. 使用确定大小的类型，如i32而不使用int类型
2. 函数入参优先使用&str代替String, 使用&[T]代替Vec
3. 尽量不使用panic!，如果需要使用，给出详细说明
4. 尽量不使用unswap 与 expect，正常情况下let与match
5. 个别字符看起是一个char，可能它是两个char。在使用时，容易产生混淆的字符，加上注释以示区别

```rust
let chars = "é".chars().collect::<Vec<_ > > ();
// U+00e9: 'latin small letter e with acute'
assert_eq!(vec!['\u{00e9}'], chars);
let chars2 = "é".chars().collect::<Vec<_ > > ();
// U+0065: 'latin small letter e',U+0301: 'combining acute accent'
assert_eq!(vec!['\u{0065}', '\u{0301}'], chars2);
```

[see](https://doc.rust-lang.org/stable/std/primitive.char.html)

6. print!或println!每次调用时会锁定stdout，如果连续使用且有性能要求时，可以手动锁定，也可以增加buffer，下面是手动锁的例子

```rust
use std::io::Write;
let mut stdout = std::io::stdout();
let mut lock = stdout.lock();
for line in lines {
writeln!(lock, "{}", line)?;
}
```

[see](https://poly000.github.io/perf-book-zh/io_zh.html)

### Pointer/Reference

7. Rust中的引用是一种特殊的指针。
    * 它由编译器来保证引用的有效性
    * 在Rust的安全代码中，所有的引用都是有效的
    * 在c++中的”引用“，是一个变量的别名，他们就是同一个对象，没有产生新的内存或对象
    * rust的引用与C++中的”引用“不是一个概念。 
    * Rust、Java、Go、Dart等语言都不支持引传参（函数参数），所以它们的引用都是类似指针。也可以这样说，不支持引用传参的语言不支持真正的引用（这里只限于我自己接触过的语言）
    * C++与C#支持引用传参数，也是支持别名方式的引用。

```rust

```

```C++

```

8. &mut T/mut &T的不同
&mut T是一个引用类型，指向个可以变的T，引用本身不可变。
mut &T是一个引用类型，引用本身可变，指向内容不可以。

```rust
let mut t = 1;
let ref1_t = &t;//ref_t1为 &T类型
// ref1_t = &t1; //不是mut &，不能二次赋值
// *ref1_t = 2;  //不是 mut t，不能二次赋值 

let ref2_t = &mut t;
// ref2_t = &t2;  //不是mut &，不能二次赋值
*ref2_t = 3;//是 mut t，能二次赋值

let mut ref_t3 = &mut t;
ref_t3 = &mut t; //是mut &，能二次赋值
*ref_t3 = 2;  //是 mut t，能二次赋值
```
9. Box::new 使用了一个神奇的单词“box”，它的作用是把对象安全的移动到heap上面，它在编译时可能会作优化以减少内存上的不必须的复制移动 ？分配两次

```rust
let d = Box::new(Data::default ());
// Data这个对被分配了两次，一次是Data::default()，在stack上, 一次是Box::new，在heap
// 有没有方法直接分配到heap上面，且是default的？
// 网上参考，有人说编译可能会优化，让它只会分配一次，[see](https://github.com/rust-lang/rust/issues/53827)，[see2](https://stackoverflow.com/questions/31502597/do-values-in-return-position-always-get-allocated-in-the-parents-stack-frame-or/31506225#31506225)
// 这个问题与C++中的placement new情况是一样的，但是没有找到在指定内存上调用default的方法，分析default的返回值是一个对象，没有转入参数的地方，它只产生一个对象，而不能在一个对象上运行。
// 所以下面是解决方法的思路（这不是一个可靠的方法，只是为了说明思路，在实际的代码中不要这样使用）
struct Data {
    name: String,
}

impl Data {
    pub fn _init(&mut self) {
        self.name = "test".to_owned();
    }
}

impl Default for Data {
    fn default() -> Self {
        let mut d = Data { name: String::default(), };
        d._init();
        d
    }
}
//这不是一个可靠的方法，只是为了说明思路，在实际的代码中不要这样使用
let mut d = unsafe {
let ptr: * mut Data = alloc(Layout::new::< Data > ()) as _;
( * ptr)._init();
Box::from_raw(ptr)
};
println!("{}", d.name);
```

9. 如果使用Pin<T>，给出足够的原因。Pin的作用是阻止使用可修改指针（&mut T）.也就是对象不可以被移动，在std中使用在在future的pull函数，解决自己引的问题。
10. trait Drop是一个trait，在对象的lifetime结束时（释放内存之前），会自动调用它的drop函数，这是编译器完成的。
    * 对于drop来说它并不释放内存，它只是在释放内存前调用的一个函数。
    * 在drop函数实现中一般会释放内存或关闭文件等清理工作，但不要弄混了，drop只是一个函数，怎么实现都是可以的。
    * drop函数一般是自动调用的，也可以手动调用，也可以阻止自动调用

```rust

```
11. 永远不要把local variable的引用赋给raw pointer；  
    double raw要二次free内存；  
    分配内存时需要认真确认是否需要zero内存，没有特别的理由都需要；  
    free岂有内存后，记得赋值为null
    
```rust
//error sample, never use it
fn err_d_raw() -> *mut *mut i32 {
    let t: *mut *mut i32 = &mut std::ptr::null_mut();
    t
}

fn ok1_d_raw<T>() -> *mut *mut T {
    let t = Box::new(std::ptr::null_mut());
    Box::into_raw(t)
}
fn ok2_d_raw<T>() -> *mut *mut T {//可以正确工作，但为了统一free内存，不建议这种方式
    let layout = std::alloc::Layout::new::<*mut *mut T>();
    //注意分配内存时，一定注意是否需要 zeroed.
    let t = unsafe { std::alloc::alloc_zeroed(layout) } as *mut *mut T;
    t
}

fn free_d_raw<T>(d: &mut *mut *mut T) {
    if *d != std::ptr::null_mut(){
        unsafe {
            let mut f =  &mut **d;//是两个*号，第一个对应的是 &mut
            if *f != std::ptr::null_mut() {
                let _ = Box::from_raw(*f);
                *f = std::ptr::null_mut();
            }
            let _ = Box::from_raw(*d);
            *d = std::ptr::null_mut();
        }
    }
}
let d1 = err_d_raw();//不要释放这个内存，它是stack，释放会产生未知错误
let mut d2 = ok1_d_raw::<i32>();
free_d_raw(&mut d2);
let mut d3 = ok2_d_raw::<i32>(); //由于使用的alloc直接分配的内存，最好使用dealloc来free内存，配对使用
free_d_raw(&mut d3);

let mut d4= ok1_d_raw::<i32>();
unsafe { *d4 = Box::into_raw(Box::new(10)); }
free_d_raw(&mut d4);

assert_ne!(d1,std::ptr::null_mut());
assert_eq!(d2, std::ptr::null_mut());
assert_eq!(d3, std::ptr::null_mut());
assert_eq!(d4, std::ptr::null_mut());

println!("err_d_raw d1: {:p}\nok1_d_raw d2: {:p}\nok2_d_raw d3: {:p}", d1, d2, d3);
```

注意  
    * "&mut std::ptr::null_mut()" 构造一个local variable(命名为 temp)，取temp的引用，转换为raw pointer。temp的lifetime只在函数内有效且是stack内存  
    * 内存的分配与释放需要配对，都使用Box，或都直接使用alloc/dealloc，不要交叉使用  
    * 统一使用Box，不直接使用alloc/dealloc，如果需要给出说明  


12. &closure会自动实现closure的FnX。  
    *. closure是一个匿名的结构体  
    *. 两个closure的代码完全相同，它们的类型也不相同  
    *. 有move的closure，也可能是Fn或FnMut的。move的含义是让closure获得拥有权，多次调用closure，所有权并没有改变  
    *. [&closure](https://github.com/rust-lang/rust/blob/master/library/core/src/ops/function.rs) 的实现，这个不在doc中  
    *. &closure与closure的效果相同，当closure传参需要获得closure所有权时，可以解决多次调用的问题。
```rust
    fn fn_once<F>(func: F) where F: FnOnce() {
        func();//只能调用一次，&func也不行
    }
    {
        let mut a = "fn_once".to_owned();
        let f = || {
            // a = capture(a);
            println!("{}", a)
        };
        let f2 = f;
        fn_once(f);//f 自动实现了 Copy,所以可以再一次使用变量f
        fn_once(f2);
    }
    {
        fn fn_once<F>(func: F) where F: FnOnce() { func() }
        let a = "fn_once move".to_owned();
        let f = move || println!("{}", a);
        f();
        (&f)();
        fn_once(&f);//可以多次调用
        fn_once(f);
        //fn_once(f); //error[E0382]: use of moved value: `f`。有move时不会实现 Copy
    }
    fn fn_<F>(func: F) where F: Fn() {
        func();
    }
    {
        let a = "fn".to_owned();
        let f = || println!("{}", a);
        let f2 = f;
        fn_(f);//f 自动实现了 Copy,所以可以再一次使用变量f
        fn_(f2);
    }
    {
        let a = "fn move".to_owned();
        let f = move || println!("{}", a);
        fn_(&f);
        fn_(f);
    }

    fn fn_mut<F>(mut func: F) where F: FnMut() {
        func();
    }
    {
        let mut a = "FnMut".to_owned();
        let mut f = || {
            a.push_str("X");
            println!("{}", a);
        };
        f();
        (&mut f)();
        fn_mut(&mut f);
        fn_mut(f);
    }
    {
        let mut a = "FnMut move".to_owned();
        let f = move || {
            a.push_str("X");
            println!("{}", a);
        };
        fn_mut(f);
        // fn_mut(f);
    }
```
下面是nightly下手动实FnX的代码
```rust
//需要增加到lib.rs文件开头
#![feature(unboxed_closures)]
#![feature(fn_traits)]

struct Data {}

impl FnOnce<()> for Data {
    type Output = ();
    extern "rust-call" fn call_once(self, args: ()) -> Self::Output {
        ()
    }
}

impl FnMut<()> for Data {
    extern "rust-call" fn call_mut(&mut self, args: ()) -> Self::Output {
        ()
    }
}

impl Fn<()> for Data {
    extern "rust-call" fn call(&self, args: ()) -> Self::Output {
        ()
    }
}
let d = Data {};
d();
(&d)();
```
13. Use impl Trait when working with Closures  
[see](https://federicoterzi.com/blog/12-rust-tips-and-tricks-you-might-not-know-yet/)  
Prefer impl Fn/FnOnce/FnMut to pass a closure to a function (called impl Trait) instead of a generic when possible to keep the signature clean. For non-trivial cases, you might need to box the closure with Box<Fn()>, but keep in mind you’ll pay an additional overhead
```rust
fn setup_teardown_generic<A: FnOnce()>(action: A) {
    println!("setting up...");
    
    action();
    
    println!("tearing down...")
}

// Use this

fn setup_teardown(action: impl FnOnce()) {
    println!("setting up...");
    
    action();
    
    println!("tearing down...")
}

// As a note, this pattern is very useful inside tests
// to create/destroy resources.

fn main() {
    setup_teardown(|| {
        println!("Action!");
    })
    
    // Output:
    // setting up...
    // Action!
    // tearing down...
}
```
14. std::Vec实现说明
    * set_len是不安全函数，要小心使用
    *

```rust

```
15. 析构函数简单可靠，不要执行不确定或费时费力或阻塞操作  
    析构函数主要是指像drop，close等方法，
16. 如果trait不希望被crate外实现，使用private::Sealed
```rust
pub trait InnerTrait: private::Sealed {
    //...
}
```    

17. derive与trait bound都可以实现时，优先使用derive，它使用更简单
```rust
// 优先使用这个
#[derive(Debug, PartialEq)]
struct Good<T> { /* ... */ }

// 不建议使用
#[derive(Debug, PartialEq)]
struct Bad<T: Debug + PartialEq> { /* ... */ }
```
18. 小心使用Copy trait。
   在绝大部分情况下都不需要实现Copy trait，因为一但实现，它会在变量赋值或传参数时自动隐式调用，这个过程很难被注意到，会产生很大的浪费。

### 多线程

1. Sync或Send只是告诉编译器是安全的，并不会做什么动着，保证安全。注意在实现时使用“unsafe”，说明需要我们自己写来保证线程的安全。  
   下面是一个反例

```rust
//这是错误代码
struct Data {
    c: Cell<i32>,
}

unsafe impl Sync for Data {}

unsafe impl Send for Data {}

impl Data {
    pub fn add(&self) {
        let t = self.c.get() + 1;
        self.c.set(t);
    }
}

let mut d = Arc::new(Data {
    c: Cell::new(0),
});

let mut d2 = d.clone();
let t2 = spawn(move | | {
    for i in 0..101 {
        d2.add();
    }
});
let mut d3 = d.clone();
let t3 = spawn(move | | {
    for i in 0..101 {
        d3.add();
    }
});
for i in 0..101 {
d.add();
}
t2.join();
t3.join();
println!("len: {}", d.c.get());//输出的结果大部分情况都不是303
```
2. Send与Sync trait编译器会自动推断为struct实现这两个trait。  
    如果struct中的所有字段都是Send或Sync的，那么这个struct也是Send或Sync的。只有编译器推断为不是Send或Sync时，我们才会手动实现它。一定注释，手机实现时是unsafe的。

3. tokio::spawn(async{})使用注意（为什么在spawn中不能使用std::sync::Mutex）  
[See tokio::spawn](https://tokio.rs/tokio/tutorial/spawning)

/// 请看下面的例子，可以自行编译看出错提示
```rust
fn sample_error1(){
    let mutex = std::sync::Mutex::<i32>::new(0);
    tokio::spawn(async move {
        let v = mutex.lock().expect("");
        async{}.await;
        println!("{}", "");
    });
}
```
编译提示1  
9   |     tokio::spawn(async move {  
|     ^^^^^^^^^^^^ future created by async block is not `Send`  

127 |         T: Future + Send + 'static,  
|                     ---- required by this bound in `tokio::spawn`  
|  
编译提示2  
= help: within `impl Future`, the trait `Send` is not implemented for `std::sync::MutexGuard<'_, i32>`  
note: future is not `Send` as this value is used across an await  
10  |         let v = mutex.lock().expect("");  
|             - has type `std::sync::MutexGuard<'_, i32>` which is not `Send`  
11  |         async{}.await;  
|         ^^^^^^^^^^^^^ await occurs here, with `v` maybe used later  
12  |         println!("{}", "");  
13  |     });  
|     - `v` is later dropped here  

编译提示1 说tokio::spawn函数要求参数为“T: Future + Send + 'static”，而提供的参数为“async block is not `Send`”不是Send的，所以就编译就不通过了。  
简单说就是函数时，参数类型不对  
编译提示2 给出 help告诉开发者更多的信息，帮助解决这个问题。  
其中“`std::sync::MutexGuard<'_, i32>` which is not `Send`”说这个不是Send，且变量v有可能在“async{}.await”之后被使用。  
这个help信息告诉我们两个解决方法  
方法1 需要一个可以Send的“MutexGuard”  
方法2 不要让变量v可能在await之后被使用。  
编译器为我们写代码真操碎了心，“怎么错的，要怎么解决”，就差把代码给写出来了。  
同时我们也产生一个疑问，std::sync::Mutex就是多线程开发的，为什么它不能在这里的多线程使用？   
分析代码的运行过程，没有安全问题，但为什么编译器不那么认为呢？这里主要在于async{}的生命周比变量v的更长，它是异步的。但是我们还使用了“await”，它并不会异步。  
从这个上看是编译器检查的太严格了或者它没有把“await”一起考虑进来，do more do better/再接再厉  
下面是按照编译器的建议给出的解决方法  
```rust
//方法1 需要一个可以Send的“MutexGuard”
//把std::sync::Mutex换成tokio::Mutex
fn sample_error1_method1(){
    let mutex = tokio::sync::Mutex::<i32>::new(0);
    tokio::spawn(async move {
        let v = mutex.lock().await;
        async{}.await;
        println!("{}", "");
    });
}
```
```rust
//方法1 不要让变量v可能在await之后被使用
fn sample_error1_method2(){
    let mutex = std::sync::Mutex::<i32>::new(0);
    tokio::spawn(async move {
        {
            let v = mutex.lock().expect("");
        }
        async{}.await;
        println!("{}", "");
    });
}
```
tokio::sync::Mutex的实现为解决std::sync::Mutex的问题做了，更多的工作，所以在使用时，能用std::sync::Mutex解决问题，就不要换了  

疑问2 编译器async{}怎么知道是否为Send呢？  
Send这个trait是自动的，block中的所有内容都可以安全的Send，那么它就会自动实现Send，否则就不实现。  

最后一定要看编译器的提示，这是最高效的解决方法

### 代码提交前准备

1. fmt --> clippy --> cargo test --no-run。这三样通过后，才提交代码
2. 清除编译警告

### 测试

1. 在assert语句中如果使用了对象的字段或函数，建议打印整个对象。
    * 检测Option为None
    ```rust
    //let data = Option ....
    assert_eq!(true,v.is_none(),"{:?}", v);
    ```
    * 检测Result为Err
    ```rust
    //let data = Result ....
    assert_eq!(true,v.is_err(),"{:?}", v);
    ```
2. 可以安装运行[miri](https://github.com/rust-lang/miri) 来测试代码

## 文档

1. 在文档中使用 Errors、Panics、Safety

```rust
/// # Errors
/// ....
/// # Panics
/// ....
/// # Safety
/// ....
/// # Examples
/// ....
```

[see](https://rust-lang.github.io/api-guidelines/documentation.html)

2. 包含指向相关内容的超链接，下面是例子

```rust
//!Just doc sample for the mod mylib::doc_
//!
//!
//! # Samples
//! ```
//! use mylib::doc_::Data;
//!
//! let modData = Data::new();
//! ```
//!

/// Just doc sample for the mylib::doc_::Data
///
///
/// # Samples
///
/// ```
/// use mylib::doc_::Data;
/// let data = Data::new();
/// ```
/// use the [`Data::new`]
///
/// use 2 the [crate::doc_::Data]
///
/// see the [`Vec`]
///
/// see1 the [Vec]
///
/// see2 the [std::vec::Vec]
///
pub struct Data {
    /// data's name
    /// see [Data::new]
    pub name: String,
    list: Vec<i32>,
}

impl Data {
    /// new struct [Data]
    /// see the field [Data::name]
    pub fn new() -> Self {
        Data {
            name: "".to_owned(),
            list: Vec::new(),
        }
    }
}
``` 

说明：

* [`Vec`]与[Vec]这两种连接方式都是可以的
* 可以使用[std::vec::Vec]/[crate::doc_::Data]全路径增加link
* 在lib.rs开头增加如下代码，会在Samples的右上角显示“Run”按钮

```rust
#![doc(html_playground_url = "https://play.rust-lang.org/")]
```

## 库使用说明

### Rbatis

1. [rbatis::tx::TxGuard] 是异步事务，如果需要不同连接之间数据的及时通知，请使用如下方式

```rust
//tx 只处理异常情况下，事务的rollback，所以会在事务提交成功后，调用 tx.manager = None; 阻止 [rbatis::tx::TxGuard]再管理事务
let mut tx = rb.begin_tx_defer(false).await?;
// .... 
rb.commit( & tx.tx_id).await?;
tx.manager = None;
```

## 参考

[The Rust Programming Language](https://doc.rust-lang.org/book/)
[The Rust Reference](https://doc.rust-lang.org/reference/index.html)
[The Rustonomicon/Rust 秘典](https://doc.rust-lang.org/nomicon/index.html)
[Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
[Rust by Example](https://doc.rust-lang.org/stable/rust-by-example/)
[The Cargo Book](https://doc.rust-lang.org/cargo/index.html)
[The rustdoc book](https://doc.rust-lang.org/rustdoc/index.html)
[Elegant Library APIs in Rust](https://deterministic.space/elegant-apis-in-rust.html)

