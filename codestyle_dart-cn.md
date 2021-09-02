[中文](./codestyle_dart-cn.md)

# Code Style -- dart 0.2

## 规则

1. 遵守软件设计六大原则
    1. 开-闭原则(Open-Closed Principle, OCP)
    2. 里氏代换原则(Liskov Substitution Principle,常缩写为.LSP)
    3. 依赖倒置原则(Dependence Inversion Principle)
    4. 口隔离原则(Interface Segregation Principle, ISP)
    5. 迪米特法则(Law of Demeter LoD)又叫做最少知识原则(Least Knowledge Principle,LKP)
    6. 单一职责原则(Simple responsibility principle SRP)
2. 功能完成，不是在自己电脑上能运行，是要整个项目能正常运行部署
3. 把问题拿出，不要把它遗忘在开发的过程中，在代码中加入 todo 说明，添加到github的issues
4. 先思考后写代码，从命名开始
5. 处理每一个error，并记录到日志中（最终写入日志中）
6. 处理所有分支，特别出现的异常情况的分支（如，不应该出现的数据等，写入error日志）
7. 重要调用都需要写入 info日志
8. 直接对外提供服务接口，必须稳定，不能因为一个错误就让整个服务停止工作
9. 对外提供的接口，统一错误编号及错误信息
10. 定义函数时要考虑两个方面，一实现函数代码是否合理，二使用是否方便，是否容易出错
11. 验证开发中的代码，使用单元测试；研究一项目技术实现等使用demo
12. 如果要使用使用全局变量，给出足够的理由
13. 提交代码的要求， 说明 格式化 编译通过，如果提交编译不通过的代码需要有特别的理由
14. 代码原则：   
    *. 函数内部结构使用数据为主线，分为三大块：定义数据，生成数据，使用数据 例子：

```dart
List<int> funName() {
  //定义数据
  var data = <int>[];
  //生成数据
      {}
  //使用数据
  return data;
}
```

## Name

常用单词[Words](./words_cn_en.md)

1. 所有源代码文件名、包名，使用小写，加下划线    
   因为有的系统不区分文件名的大小写，如windows(在window10中可以手动设置区分大小写)，所以为了统一一至，文件命名使用小写+下划线
2. 所有目录文件名，使用小写，加下划线 因为有的系统不区分文件名的大小写，如windows(在window10中可以手动设置区分大小写)，所以为了统一一至，文件命名使用小写+下划线
3. 命名使用有明确函义的英文单词
4. 类型命名使用驼峰，常量值const命名用驼峰
5. 下划线开头的为私有
6. 首字母缩写的词，命名要大写
7. 不使用数据库及dart的关键字或保留字命名，如不能使用for来命名一个字段名

## 目录文件

1. 所有的demo放入“ 仓库名/demo ” 目录中
2. 如果是框架或基础库，需要“仓库名/sample”

## 代码

1. array是List
```
main() {
  var list = [1,2,3];//这里使用var是为了说明 array is list
  print( list is List ); // true
}
```

2. 尽量使用明确的数据类型，尽量少使用dynamic类型
3. 减少关键字var的使用，要使用明确的类型，要么使用dynamic.
4. dart中并没有public、protected、private等关键字，声明变量与方法时，前面加上 "_" 即可作为private方法使用.  
   注意： "_" 的限制范围并不是类级别，而是库
5. implements,extends,abstract,class,mixin混入, Extension   
   interface关键字已经被移除了   
   implements abstractName,className: 可以是abstract或class，可以多个，也就是多实现。实现只会包含实例字段与实例方法，其中实例字段变为get与set方法，所以实现的全是抽象方法；final实例字段与普通实例字段一样   
   extends abstractName/className: 可以是abstract或class，但只能有一个，也就是单继承 with  
   minxinName1,minxinName2：mixin，可以多个 mixin ... on Name1，Name2: on后面指这个mixin只能使用在指定的classNames上。      

implements与extends的区别是，implements只有抽象方法，extends包含字段,且不会把字段转换为get set。  
mixin与extends的区别是，mixin with可以多个，有点像“继承”，但mixin是编译时就确定调用方法的，可以说是编译时 多太。而extends是继承，且是运行时多态   

-|extends|implements|mixin|-
----|----|----|----|----
单个|是     |不        |不
转字段为get set|否|是|否| 把字段转换为get set方法
运行多态|是|是|否
编译多态|否|否|是
纯抽象方法|否|是|否|方法没有实现体的
代码后覆盖|否|否|是 | 方法同名时，出现在后面的会覆盖前面的，在调用方法时会调用到后面的，前的被覆盖了。  
下面是使用建议及规则  
    1. 会使用到同一个对象上的mixin不要重名，如果有，给出足够的理由 
    2. 如果一个class会被用在implements之后，就以interface的方式来设计，需要说明为什么要这样设计

6. “~/” 除，但返回值是整数
7. 相等比较
   判两对象是否为 同一个对象，用identical（）方法。
8. 操作符 ??=,??,?,!
    - a??=value, 如果a为null，则赋值value给a;如果不为null，则a不变
    - 条件表达式 常见表达式 term ? expr1 : expr2
    - expr1 ?? expr2，如果expr1不为返回，否则计算并返回expr2
    - ?还可以使用在对象的成员上，表示如果对象为null就返回null
    - Object? name, 使用在类型后面定义一个变量，表示变量可以为空
    - name!，表示name一定不为空，且返回非空值。 在运行时如果name为null会抛出异常
9. 对象级联操作符
“..”一个对象上，多次调用该对象的多个方法或成员
```dart
   new Person()
        ..name = "not6"
        ..age = "110"
        ..saySomething();
```
10. 异常
Exception和Error两个类型。  
```dart
   try { 
        //...
    } on Exception catch (e) {    // catch 捕获所有异常
        print('Unknown exception: $e');
    }
    try { 
        //...
    } catch (e,s) {               // catch 可以带有一个或两个参数，第二个为堆栈信息
        print('Unknown exception: $e');
    }
```

11. 异步dart-future和Microtask执行顺序

- Dart 中事件的执行顺序：Main > MicroTask > EventQueue

```dart
    void testSX(){
      new Future(() => print('s_1'));
      scheduleMicrotask(() => print('s_2'));
      print('s_3');
    }

```
结果：  
    I/flutter (32415): s_3  
    I/flutter (32415): s_2  
    I/flutter (32415): s_1  
12. future 最主要的功能就是提供了链式调用 多个future的执行顺序

- 规则一：Future 的执行顺序为Future的在 EventQueue 的排列顺序。类似于 JAVA 中的队列，先来先执行。
- 规则二：当任务需要延迟执行时，可以使用 new Future.delay() 来将任务延迟执行。
- 规则三： Future 如果执行完才添加 than ，该任务会被放入 microTask，当前 Future 执行完会执行 microTask，microTask
  为空后才会执行下一个Future。
- 规则四：Future 是链式调用，意味着Future 的 then 未执行完，下一个then 不会执行。

```dart
    //多组类型，代码示例：
    void testFuture() {
      Future f1 = new Future(() => print('f1'));
      Future f2 = new Future(() =>  null);
      Future f3 = new Future.delayed(Duration(seconds: 1) ,() => print('f2'));
      Future f4 = new Future(() => null);
      Future f5 = new Future(() => null);

      f5.then((_) => print('f3'));
      f4.then((_) {
        print('f4');
        new Future(() => print('f5'));
        f2.then((_) {
          print('f6');
        });
      });
      f2.then((m) {
        print('f7');
      });
      print('f8');
    }

```
输出结果：  
       com.example.flutter_dart_app I/flutter: f8  
       com.example.flutter_dart_app I/flutter: f1  
       com.example.flutter_dart_app I/flutter: f7  
       com.example.flutter_dart_app I/flutter: f4  
       com.example.flutter_dart_app I/flutter: f6  
       com.example.flutter_dart_app I/flutter: f3  
       com.example.flutter_dart_app I/flutter: f5  
       com.example.flutter_dart_app I/flutter: f2  
说明：  
      首先执行Main 的代码，所以首先输出: 8;  
      然后参考上面的规则1，Future 1 到 5 是按初始化顺序放入 EventQueue中，所以依次执行Future 1到5 ， 所以输出结果：8，1，7。  
      参考规则2，f3 延时执行，一定是在最后一个：8，1，7，…，2。  
      在 f4 中，首先输出 f4 ：8，1，7，4，…，2。  
      在 f4 的 then 的方法块中，新建了Future, 所以新建的 Future 将在 EventQueue尾部，最后被执行：8，1，7，4，…，5，2。  
      在 f4 的 then 的方法块中，给 f2 添加了 then ,但此时 f2 已经执行完了，参考规则三，所以 then 中的代码会被放到 microTask 中，在当前 Future 执行完后执行。 因为此时Future f4已经执行完了，所以会处理microTask（microTask优先级高）。结果：8，1，7，4，6，..，5，2。  
      此时我们的 EventQueue 中还有 f5，和在 f4 中添加的新的Future。 所以我们的最终结果就是：8，1，7，4，6，3，5，2。  
13. 多future和多micTask的执行顺序

- 与nodejs中的机制非常类似,NoBlock主线程模型

```dart
      //代码示例：
      void testScheduleMicrotatsk() {
        scheduleMicrotask(() => print('Mission_1'));
      //注释1
        new Future.delayed(new Duration(seconds: 1), () => print('Mission_2'));
      //注释2
        new Future(() => print('Mission_3')).then((_) {
          print('Mission_4');
          scheduleMicrotask(() => print('Mission_5'));
        }).then((_) => print('Mission_6'));
      //注释3
        new Future(() => print('Mission_7'))
            .then((_) => new Future(() => print('Mission_8')))
            .then((_) => print('Mission_9'));
      //注释4
        new Future(() => print('Mission_10'));
        scheduleMicrotask(() => print('Mission_11'));
        print('Mission_12');
      }
```
输出结果：  
     I/flutter (19025): Mission_12  
     I/flutter (19025): Mission_1  
     I/flutter (19025): Mission_11  
     I/flutter (19025): Mission_3  
     I/flutter (19025): Mission_4  
     I/flutter (19025): Mission_6  
     I/flutter (19025): Mission_5  
     I/flutter (19025): Mission_7  
     I/flutter (19025): Mission_10  
     I/flutter (19025): Mission_8  
     I/flutter (19025): Mission_9  
     Syncing files to device MIX 3...  
     I/flutter (19025): Mission_2  

结果分析：  
     根据 Main > MicroTask > EventQueue。我们首先会得到输出结果：12，1，11。  
     注释1 的 Future 是延时执行，所以：12，1，11，…，2。  
     注释2 中创建了 Microtask，Microtask会在该Future执行完后执行，所以：12，1，11，4，6，5，…，2。  
     重点来了: 我们在注释3 的Future 的 then 中新建了Future(输出Mission_8),新建的 Future 将被加到 EventQueue尾部，并且，注释3的Future后续的then将不再执行，因为这个链被阻塞了！  
     注意对比上一题中的 f4, 上一题中的 f4 是一个 than 方法包裹了代码块。  
     此时的结果：12，1，11，4，6，5，7，…，2。  
     执行完注释4 的 Future，然后会执行我们在注释3 Future 新加入的 Future，之后注释3 的Future不再阻塞，会继续执行，结果： 12，1，11，4，6，5，7，10，8，9，2。  

14. 定义变量时，类型是确定的就给出具体的类型

```dart

var mx = {}; //不要这样定义map
var m = <String, String>{}; //使用这种方式
Map<String, String> m = {}; //这种方式也可以，上面的方式少写map字符，Dart官方推荐使用，这里都可以使用

var a = <String>[];
List<String> a = [];
```

15. 小心使用?, 避免非空时的异常 在不确定对象是否为空时可以使用?简化代码，但不要误以为?一定不会抛出异常

```dart

var a = <String>[];
//.....
var first = a?.first;
//1,如果a为null，那么first为null
//2,如果a不为空存，那么调用first函数。特别注意如果a数组的长度为零时，它会抛出异常
```

## 库

### source_gen

1. 做单元测试时增加 reader参数，不然导入库全为dynamic类型

```dart
testBuilder
(
//...
reader: await
PackageAssetReader.currentIsolate()
, //如果不加这个参数导入库全为dynamic类型
//...
```

### ffi

1. ffi.allocate函数分配内存，没有把内存清零 注：window debug下发现的结构，没有验证release或其它系统下面也有这个问题 下面是清零的代码

```dart
Pointer<T> allocateZero<T extends NativeType>({int count = 1}) {
  final int totalSize = count * sizeOf<T>();
  var ptr = ffi.allocate<T>();
  var temp = ptr.cast<Uint8>();
  for (var i = 0; i < totalSize; i++) {
    temp
        .elementAt(i)
        .value = 0;
  }
  return ptr;
}
```

