
- [1. 新时代的语言](#1-新时代的语言)
- [2. 语言精要](#2-语言精要)
  - [2.1. Rust 语言的基本构成](#21-rust-语言的基本构成)
    - [2.1.1. 语言规范](#211-语言规范)
    - [2.1.2. 编译器](#212-编译器)
    - [2.1.3. 核心库](#213-核心库)
    - [2.1.4. 标准库](#214-标准库)
    - [2.1.5. 包管理器](#215-包管理器)
  - [2.2. 语句与表达式](#22-语句与表达式)
  - [2.3. 变量与绑定](#23-变量与绑定)
    - [2.3.1. 位置表达式和值表达式](#231-位置表达式和值表达式)
    - [2.3.2. 不可变绑定与可变绑定](#232-不可变绑定与可变绑定)
    - [2.3.3. 所有权与引用](#233-所有权与引用)
  - [2.4. 函数与闭包](#24-函数与闭包)
    - [2.4.1. 函数定义](#241-函数定义)
    - [2.4.2. 作用域与生命周期](#242-作用域与生命周期)
    - [2.4.3. 函数指针](#243-函数指针)
    - [2.4.4. CTFE机制](#244-ctfe机制)
    - [2.4.5. 闭包](#245-闭包)
  - [2.5. 流程控制](#25-流程控制)
    - [2.5.1. 条件表达式](#251-条件表达式)
    - [2.5.2. 循环表达式](#252-循环表达式)
    - [2.5.3. match表达式与模式匹配](#253-match表达式与模式匹配)
    - [2.5.4. if let 和 while let 表达式](#254-if-let-和-while-let-表达式)
  - [2.6. 基本数据类型](#26-基本数据类型)
    - [2.6.1. 布尔类型](#261-布尔类型)
    - [2.6.2. 基本数字类型](#262-基本数字类型)
    - [2.6.3. 字符类型](#263-字符类型)
    - [2.6.4. 数组类型](#264-数组类型)
    - [2.6.5. 范围类型](#265-范围类型)
    - [2.6.6. 切片类型](#266-切片类型)
    - [2.6.7. str 字符串类型](#267-str-字符串类型)
    - [2.6.8. 原生指针](#268-原生指针)
    - [2.6.9. never 类型](#269-never-类型)
  - [2.7. 复合数据类型](#27-复合数据类型)
    - [2.7.1. 元组](#271-元组)
    - [2.7.2. 结构体](#272-结构体)
    - [2.7.3. 枚举体](#273-枚举体)
  - [2.8. 常用集合类型](#28-常用集合类型)
    - [2.8.1. 线性序列：Vec](#281-线性序列vec)
    - [2.8.2. 线性序列：双端队列](#282-线性序列双端队列)
    - [2.8.3. 线性序列：链表](#283-线性序列链表)
    - [2.8.4. Key-Value映射表：HashMap和BTreeMap](#284-key-value映射表hashmap和btreemap)
    - [2.8.5. 集合：HashSet和BTreeSet](#285-集合hashset和btreeset)
    - [2.8.6. 优先队列：BinaryHeap](#286-优先队列binaryheap)
  - [2.9. 智能指针](#29-智能指针)
  - [2.10. 泛型和 trait](#210-泛型和-trait)
    - [2.10.1. 泛型](#2101-泛型)
    - [2.10.2. trait](#2102-trait)
  - [2.11. 错误处理](#211-错误处理)
  - [表达式优先级](#表达式优先级)
  - [注释与打印](#注释与打印)
# 1. 新时代的语言
# 2. 语言精要
## 2.1. Rust 语言的基本构成
1. 语言规范
2. 编译器
3. 核心库
4. 标准库
5. 包管理器
### 2.1.1. 语言规范
Rust语言规范主要由Rust语言参考（The Rust Reference）和RFC文档共同构成。
### 2.1.2. 编译器
Rust官方的编译器叫rustc，负责将Rust源代码编译为可执行文件或其他库文件（.a、.so、.lib、.dll等）
- rustc有如下特点：
    - 跨平台
    - 支持交叉编译
    - 使用 LLVM 作为编译器后端，具有很好的代码生成和优化技术，支持多个目标平台
    - rustc是用Rust语言开发的，包含在Rust语言源码中。
### 2.1.3. 核心库
核心库中定义的是Rust语言的核心，不依赖于操作系统和网络等相关的库，甚至不知道堆分配，也不提供并发和I/O。
- 可以通过在模块顶部引入`＃![no_std]`来使用核心库，`#[no_std]`来明确不需要标准库。
- Rust会为每个crate都自动引入标准库模块，除非使用＃[no_std]属性明确指定了不需要标准库
- 核心库和标准库的功能有一些重复，包括如下部分：
    - 基础的trait，如Copy、Debug、Display、Option等。
    - 基本原始类型，如bool、char、i8/u8、i16/u16、i32/u32、i64/u64、isize/usize、f32/f64、str、array、slice、tuple、pointer等
    - 常用功能型数据类型，满足常见的功能性需求，如String、Vec、HashMap、Rc、Arc、Box等
    - 常用的宏定义，如println！、assert！、panic！、vec！等
- 做嵌入式应用开发的时候，核心库是必需的。
### 2.1.4. 标准库
Rust标准库提供应用程序开发所需要的基础和跨平台支持
- 标准库包含的内容大概如下：
    - 与核心库一样的基本trait、原始数据类型、功能型数据类型和常用宏等，以及与核心库几乎完全一致的API
    - 并发、I/O和运行时。例如线程模块、用于消息传递的通道类型、Sync trait等并发模块，文件、TCP、UDP、管道、套接字等常见I/O。
    - 平台抽象。底层操作接口，比如 std：：mem、std：：ptr、std：：intrinsics 等，操作内存、指针、调用编译器固有函数。
    - 可选和错误处理类型Option和Result，以及各种迭代器等。
### 2.1.5. 包管理器
把按一定规则组织的多个rs文件编译后就得到一个包（crate）。包是Rust代码的基本编译单元，也是程序员之间共享代码的基本单元。
- Rust提供了非常方便的包管理器Cargo
    - `cargo new` 命令默认可以创建一个用于编写可执行二进制文件的项目
        - `cargo new --lib lib_crate` 添加--lib参数，则可以创建用于编写库的项目
    - `cargo build` 对项目进行编译
    - `cargo run` 运行
## 2.2. 语句与表达式
Rust 中的语法可以分成两大类：`语句（Statement）和表达式（Expression）`。语句是指要执行的一些操作和产生副作用的表达式。表达式主要用于计算求值。
- 语句
  - 声明语句（ Declaration statement）
    - 声明各种语言项（Item），包括声明变量、静态变量、常量、结构体、函数等，以及通过extern和use关键字引入包和模块等
  - 表达式语句（ Expressionstatement）
    - 特指以分号结尾的表达式。`此类表达式求值结果将会被舍弃，并总是返回单元类型（）`
- 以叹号结尾，并且可以像函数一样被调用的语句，在Rust中叫作宏，如：`println!()` `assert_eq!`
- Rust编译器在解析代码的时候，如果碰到分号，就会继续往后面执行；如果碰到语句，则执行语句；如果碰到表达式，则会对表达式求值，如果分号后面什么都没有，就会补上单元值（）。
- 当遇到函数的时候，会将函数体的花括号识别为块表达式（Block Expression）。块表达式是由一对花括号和一系列表达式组成的，它总是返回块中最后一个表达式的值。
- **可以将Rust看作一切皆表达式。由于当分号后面什么都没有时自动补单元值（）的特点，我们可以将 Rust 中的语句看作计算结果均为（）的特殊表达式。而对于普通的表达式来说，则会得到正常的求值结果。**
```rust
fn main() {
    pub fn anwser() -> () { // 返回值是 单元 类型，单元类型拥有唯一的值，就是它本身，它表示“没有什么特殊的价值”
        let a = 40; // 语句
        let b = 2; // 语句 
        assert_eq!(sum(a, b), 42); // 宏语句
    }
    pub fn sum(a: i32, b: i32) -> i32 {
        a + b // 表达式，返回表达式的结果
    }
    answer(); // 最后一个语句，分号后面什么都没有，会补上单元值
    // main 返回 单元值 ()
}
```
## 2.3. 变量与绑定
通过let关键字来创建变量，这是Rust语言从函数式语言中借鉴的语法形式。let创建的变量一般称为绑定（Binding），它表明了标识符（Identifier）和值（Value）之间建立的一种关联关系.
### 2.3.1. 位置表达式和值表达式
- 位置表达式（ Place Expression）-> 左值
  - **位置表达式就是表示内存位置的表达式**。
  - 分别有以下几类
    - 本地变量
    - 静态变量
    - 解引用（*expr）
    - 数组索引（expr[expr]）
    - 字段引用（expr.field）
    - 位置表达式组合
  - **通过位置表达式可以对某个数据单元的内存进行读写。主要是进行写操作，这也是位置表达式可以被赋值的原因。**
- 值表达式（ ValueExpression）-> 右值
  - 不是位置表达式的就是值表达式。
  - 值表达式一般只**引用**了某个存储单元地址中的数据。它相当于数据值，**只能进行读操作**。
- `所以，能进行写的是位置表达式，只能进行读的是值表达式？`
- 从语义角度来说，**位置表达式代表了持久性数据，值表达式代表了临时数据**。位置表达式一般有持久的状态，值表达式要么是字面量，要么是表达式求值过程中创建的临时值。
- 表达式的求值过程在不同的上下文中会有不同的结果。`什么是上下文？`
  - 位置上下文
    - 赋值或者复合赋值语句左侧的**操作数**
      - `许多表达式包含子表达式，称为表达式的操作数。`
      - 操作数可以出现在位置上下文或值上下文中。表达式的计算取决于它自己的类别和它出现的上下文。
    - 一元引用表达式的独立操作数，eg: 
      - `let x = &a` x 是位置上下文，&a 把赋值语句的右侧变成了位置上下文，只是共享内存地址
    - 包含隐式借用（引用）的操作数
    -  match判别式或let绑定右侧在使用ref模式匹配的时候也是位置上下文
  - 值上下文
    - 不是位置上下文的，就是值上下文，如 函数、常量
    - 值表达式不能出现在位置上下文中
### 2.3.2. 不可变绑定与可变绑定
- 使用let关键字声明的位置表达式默认不可变，为不可变绑定，可读不可写
- 而 let mut声明的可变绑定则是可以对相应的存储单元进行写入的
```rust
fn main() {
    let a = 1;
    // a = 2; // immutabel and error
    let mut b = 2;
    b = 3; // mutable
}
```
### 2.3.3. 所有权与引用
当**位置表达式**出现在**值上下文中**时，该位置表达式将会把内存地址转移给另外一个位置表达式，这其实是所有权的转移
- 在语义上，每个变量绑定实际上都拥有该存储单元的所有权，这种转移内存地址的行为就是所有权（OwnerShip）的转移，在 Rust 中称为`移动（Move）语义`，**那种不转移的情况实际上是一种复制（Copy）语义**
- Rust没有GC，所以完全依靠所有权来进行内存管理
- Rust提供引用操作符（&），可以**直接获取表达式的存储单元地址，即内存位置**。可以通过该内存位置对存储进行读取
```rust
    let a = [1,2,3];
    let b = &a; // 引用操作符&取得a的内存地址，使用引用操作符已经将赋值表达式右侧变成了位置上下文，它只是共享内存地址；读借用
    println!("{:p}", b);
    let mut c = vec![1, 2, 3];
    let d = &mut c; // 通过&mut获取c的可变引用，赋值给d; 写借用
    d.push(4); // 改变可变数组，末尾push4
    println!("{:?}", d); // [1, 2, 3, 4]
    println!("{:?}", c); // [1, 2, 3, 4]
    let e = &42;
    assert_eq!(42, *e);
```
- 从语义上来说，不管是&a还是&mut c，都相当于对a和c所有权的借用，因为a和c还依旧保留它们的所有权，所以引用也被称为`借用`
## 2.4. 函数与闭包
### 2.4.1. 函数定义
- 函数是通过关键字`fn`定义的
- 参数与返回值类型需要严格遵守
- 函数体是由花括号括起来的，它实际上是一个块表达式，最终只返回块中最后一个表达式的求值结果。如果想提前返回，则需要使用return关键字
### 2.4.2. 作用域与生命周期
Rust 语言的作用域是静态作用域，即词法作用域（Lexical Scope）。由一对花括号来开辟作用域，其作用域在词法分析阶段就已经确定了，不会动态改变
- 生命周期（LifeTime）。变量绑定的生命周期总是遵循这样的规律：从使用 let 声明创建变量绑定开始，到超出词法作用域的范围时结束。

### 2.4.3. 函数指针
- 函数为一等公民。这意味着，函数自身就可以作为函数的参数和返回值使用

### 2.4.4. CTFE机制
CTFE 编译时函数执行
```rust
// #![feature(const_fn)] // Rust 2018 版本后不需要加该特性
const fn init_len() -> usize() {
    return 5;
}
fn main() {
    len arr = [0; init_len()];
}
```
- Rust 中固定长度的数组必须在编译期就知道长度，否则会编译出错
- 使用const fn定义的函数，必须可以确定值，不能存在歧义
- Rust中的CTFE是由miri来执行的。miri是一个MIR解释器，目前已经被集成到了Rust编译器 rustc中
### 2.4.5. 闭包
闭包也叫匿名函数
- 闭包有以下几个特点：
  - 可以像函数一样被调用
  - 可以捕获上下文环境中的自由变量
  - 可以自动推断输入和返回的类型
```rust
fn main() {
    let out = 42;
    // fn add(i: i32, j: i32) -> i32 {i + j + out}; // can't capture dynamic environment in a fn item
    fn add(i: i32, j: i32) -> i32 {i + j};
    
    // 闭包，可以捕获上下文环境中的自由变量
    let closure_annotated = |i: i32, j: i32| -> i32 {i + j + out}; 
    // 自动推断输入和返回的类型
    let closure_inferred = |i, j| i + j + out;
    let i = 1;
    let j = 2;
    assert_eq!(3, add(i, j));
    assert_eq!(45, closure_annotated(i, j));
    assert_eq!(45, closure_inferred(i, j));
}
```
- **闭包和函数有一个重要的区别，那就是闭包可以捕获外部变量，而函数不可以**，其他语言函数可以捕获外部变量。
- `Rust中闭包实际上就是由一个匿名结构体和trait来组合实现的，但编译器帮我们自动转换了`
```rust
// 闭包作为参数的情况
fn main() {
    let a = 2;
    let b = 3;
    assert_eq!(closure_math(|| a + b), 5);
    assert_eq!(closure_math(||a * b), 6);
}

fn closure_math<F: Fn() -> i32>(op: F) -> i32 { // Fn() -> i32 是 闭包类型
    op()
}
```
- 闭包捕获借用要保证借用唯一
```Rust
#![allow(unused)]
fn main() {
let mut b = false;
let x = &mut b; // &&mut
{
    let mut c = || { *x = true; }; // 闭包捕获了 x，要保证x唯一被借用，x不能被借用了
    // The following line is an error:
    // let y = &x; // 仍在 闭包的生命周期中，x 不能被 借用
    c();
}
let z = &x; // 闭包生命周期外，x 又可以被借用了
}
```
- 闭包做返回值
```rust
// 在函数定义时并不知道具体的返回类型，但是在函数调用时，编译器会推断出来。这个过程也是零成本抽象的，一切都发生在编译期
fn two_times_impl() -> impl Fn(i32) -> i32 { // 使用了impl Fn（i32）-＞i32作为函数的返回值，它表示实现Fn（i32）-＞i32 的类型
    let i = 2;
    move |j| j * i // 闭包的捕获会优先使用借用，使用 move 关键字 将捕获变量i的所有权转移到闭包中，就不会按引用进行捕获变量，这样闭包才可以安全地返回
}

fn main() {
    let result = two_times_impl();
    assert_eq!(result(2), 4)
}
```
## 2.5. 流程控制
### 2.5.1. 条件表达式
### 2.5.2. 循环表达式
- `while true` 与 `if true` 有缺陷，无限循环请使用`loop`
  ```rust
  // 编译器会报错，编译器只知道while true循环返回的是单元值
  fn while_true(x: i32) -> i32 {
      while true {
          return x + 1
      }
  }
  fn main() {
      let y = while_true(5);
      assert_eq!(y, 6);
  }
  ```
  - Rust编译器在对while循环做流分析（Flow Sensitive）的时候，不会检查循环条件，编译器会认为 while 循环条件可真可假，所以循环体里的表达式也会被忽略，此时编译器只知道while true循环返回的是单元值，而函数返回的是i32，其他情况一概不知。这一切都是因为 CTFE 功能的限制，while 条件表达式无法作为编译器常量来使用。只有等将来CTFE功能完善了，才可以正常使用。同理，if true在只有一条分支的情况下，也会发生类似情况。

### 2.5.3. match表达式与模式匹配
```rust
fn main() {
    let number = 42;
    match number {
        0 => println!("origin"),
        // 1...3 => println!("all"),
        | 5 | 7 | 13 => println!("bad luck"),
        n @ 42 => println!("answer is {}", n),
        _ => println!("common"), // match 需要全覆盖，如果覆盖不了，需要缺省
    }
}
```
- 使用操作符@可以将模式中的值绑定给一个变量，供分支右侧的代码使用，这类匹配叫**绑定模式（BindingMode）**
- match表达式必须穷尽每一种可能，所以一般情况下，会使用通配符_来处理剩余的情况
- match分支左边就是模式，右边就是执行代码
- 所有分支必须返回同一个类型，但是左侧的模式可以是不同的。

### 2.5.4. if let 和 while let 表达式
```rust
fn main() {
    let boolean = true;
    let mut binary = 0;
    if let true = boolean {
        binary = 1;
    }

    assert_eq!(binary, 1)
}
```
- if let左侧为模式，右侧为要匹配的值
```rust
fn main() {
   let mut v = vec![1, 2, 3, 4, 5];
   while let Some(x) = v.pop() { // 调用v的pop方法会返回Option类型; 左侧Some（x）为匹配模式，它会匹配右侧pop方法调用返回的Option类型结果，并自动创建x绑定
       println!("{}", x);
   }
}
```

## 2.6. 基本数据类型
### 2.6.1. 布尔类型
### 2.6.2. 基本数字类型
- 基本数字类型分为三类：
  - 固定大小的类型
    - 无符号整数
    - 符号整数
  - 动态大小的类型
    - usize 数值范围为0～232-1或0～264-1，占用4个或8个字节，具体取决于机器的字长。
    - isize 数值范围为-231～231-1或-263～263-1，占用4个或8个字节，同样取决于机器的字长
  - 浮点数
    - f32
    - f64
```rust
fn main() {
    let num = 42u32;
    let num = 0x2A; // 十六进制
    let num = 0o106; // 八进制
    let num = 0b1101_1011; // 二进制
    let num = 0b11011111;
    assert_eq!(b'*', 42u8); //  字节字面量
    assert_eq!(b'\'', 39u8);
    println!("{:?}", std::f32::INFINITY);
    println!("{:?}", std::f32::NEG_INFINITY);
    println!("{:?}", std::f32::NAN);
    println!("{:?}", std::f32::MIN);
    println!("{:?}", std::f32::MAX);
}
```
- 数字字面量后面可以直接使用**类型后缀**，比如42u32，代表这是一个u32类型
- 不加后缀或者没有指定类型，**Rust编译器会默认推断数字为i32类型**
- 支持**字节字面量**

### 2.6.3. 字符类型
- 使用单引号来定义字符（Char）类型，是一个Unicode标量值，每个字符占4个字节
- 可以使用as操作符将字符转为数字类型 `assert_eq!('%' as i8, 37)`

### 2.6.4. 数组类型
- 数组（Array）是Rust内建的原始集合类型，数组的特点为
  - 数组大小固定
  - 元素均为同类型
  - 默认不可变
- 数组的类型签名为[T；N]。T是一个泛型标记，代表具体类型；N 代表数组的长度
- 对于原始固定长度数组，只有实现Copy trait的类型才能作为其元素
  - 只有可以在栈上存放的元素才可以存放在该类型的数组中

### 2.6.5. 范围类型
- Rust 内置了范围（Range）类型，包括左闭右开和全闭两种区间
  ```rust
  fn main() {
      assert_eq!((1..5), std::ops::Range{start: 1, end: 5});
      assert_eq!((1..=5), std::ops::RangeInclusive::new(1, 5));
      assert_eq!(3+4+5, (3..6).sum());
      assert_eq!(3+4+5+6, (3..=6).sum());
      for i in 1..5 {
          print!("{} ", i);
      }
      println!("");
      for i in 1..=5 {
          print!("{} ", i);
      }
  }
  ```
  - (1..5）表示左闭右开区间，（1..=5）则表示全闭区间。它们分别是std::ops::Range 和 std::ops::RangeInclusive 的实例
  - 范围自带了一些方法，比如 sum，可以为范围中的元素进行求和
  - 并且每个范围都是一个迭代器，可以直接使用 for 循环进行打印

### 2.6.6. 切片类型
切片（Slice）类型是对一个数组（包括固定大小数组和动态数组）的引用片段，有利于安全有效地访问数组的一部分，而不需要拷贝。因为理论上讲，切片引用的是已经存在的变量。在底层，切片代表一个指向数组起始位置的指针和数组长度。用[T]类型表示连续序列，那么切片类型就是&[T]和&mut[T]。
```rust
fn main() {
    let arr: [i32; 5] = [1, 2, 3, 4, 5];

    assert_eq!(&arr, &[1, 2, 3, 4, 5]);
    assert_eq!(arr[1..], [2, 3, 4, 5]);
    assert_eq!((&arr).len(), 5);
    assert_eq!((&arr).is_empty(), false);
    let arr = &mut [1, 2, 3];
    arr[1] = 7;
    assert_eq!(arr, &[1, 7, 3]);
    let vec = vec![1,2,3];
    assert_eq!(&vec[..], [1, 2, 3]);
}
```
- 可以理解为：对数组内容的引用就是切片？

### 2.6.7. str 字符串类型
- Rust提供了原始的字符串类型str，也叫作字符串切片。它通常以不可变借用的形式存在，即&str
- 出于内存安全的考虑，Rust将字符串分为两种类型：
  - 一种是固定长度字符串，不可随便更改其长度，就是str字符串；
  - 另一种是可增长字符串，可以随意改变其长度，就是String字符串。
```rust
fn main() {
    let trust: &'static str = "Rust 是一门优雅的语言"; // 5 + 3 * 8  定义了字符串字面量trust 本质上，字符串字面量也属于str类型，只不过它是静态生命周期字符串&＇static str
    let ptr = trust.as_ptr();
    let len = trust.len();
    assert_eq!(29, len);
    let s = unsafe {
        let slice = std::slice::from_raw_parts(ptr, len);
        std::str::from_utf8(slice)
    };
    assert_eq!(s, Ok(trust));
}
```
- str字符串类型由两部分组成：指向字符串序列的指针和记录长度的值。可以通过str模块提供的as_ptr和len方法分别求得指针和长度
- Rust中的字符串本质上是一段有效的UTF8字节序列。所以，可以将一段字节序列转换为str字符串

### 2.6.8. 原生指针
- 将可以表示内存地址的类型称为指针
  - 引用（Reference）
    - 引用，它本质上是一种非空指针
    - 引用主要应用于Safe Rust中，在Safe Rust中，编译器会对引用进行借用检查，以保证内存安全和类型安全
  - 原生指针（Raw Pointer）
    - 主要用于Unsafe Rust中，直接使用原生指针是不安全的，比如原生指针可能指向一个Null，或者一个已经被释放的内存区域
    - Rust支持两种原生指针
      - 不可变原生指针*const T
      - 可变原生指针*mut T
      ```rust
      fn main() {
          let mut x = 10;
          let ptr_x = &mut x as *mut i32; // 通过 as 操作符将&mut x 可变引用转换为*mut i32 可变原生指针ptr_x
          let y = Box::new(20);
          let ptr_y = &*y as *const i32;
          unsafe {
              *ptr_x += *ptr_y; // 对ptr_x和ptr_y指针解引用，并将两个指针指向的值求和
          }
          assert_eq!(x, 30);
      }
      ```
  - 函数指针（fn Pointer）
  - 智能指针（Smart Pointer）

### 2.6.9. never 类型
- Rust中提供了一种特殊数据类型，never类型，即!
  - 该类型用于表示永远不可能有返回值的计算类型
- never类型是可以强制转换为其他任何类型的。

## 2.7. 复合数据类型
- 元组（Tuple）
- 结构体（Struct）
- 枚举体（Enum）
- 联合体（Union）

### 2.7.1. 元组
- 元组用一对 `( )` 包括的一组数据，**可以包含不同种类的数据**
```rust
fn move_coords( x: (i32, i32) ) -> (i32, i32) {
    (x.0 + 1, x.1 + 1)
}

fn main() {
    let tuple: (&'static str, i32, char) = ("hello", 5, 'c');
    assert_eq!(tuple.0, "hello");
    let coords = (0, 1);
    let result = move_coords(coords);
    assert_eq!(result, (1, 2));
    let (x, y) = move_coords(coords); // let支持模式匹配，可以用来解构元组
    assert_eq!(x, 1);
    assert_eq!(y, 2);
}
```
- 可以通过索引来获取元组内元素的值
- 因为let支持模式匹配，所以可以用来解构元组
- 利用元组也可以让函数返回多个值
- 当元组中只有一个值的时候，需要加逗号，即 （0，），这是为了和括号中的其他值进行区分
- `()` 空元组

### 2.7.2. 结构体
- Rust 提供三种结构体
  - 具名结构体
  - 元组结构体
    - 特点是，字段没有名称，只有类型 `struct Color(i32, i32, i32`
    - 当一个元组结构体只有一个字段的时候，我们称之为`New Type模式`，`struct Integer(u32)`
  - 单元结构体 
    - 没有任何字段的结构体 `struct Empty`
    - 单元结构体实例就是其本身
      - 在 Release 编译模式下，单元结构体实例会被优化为同一个对象；而在 Debug模式下，则不会进行这样的优化
-  构体名称要遵从驼峰式命名规则。虽然不按驼峰式命名也可以通过编译，但是编译器会警告你
-  结构体上方的＃[derive（Debug，PartialEq）]是属性，可以让结构体自动实现Debug trait和PartialEq trait，它们的功能是允许对结构体实例进行打印和比较
-  在impl块中定义的函数被称为方法，这和面向对象有点渊源

### 2.7.3. 枚举体
- 枚举体（Enum，也可称为枚举类型或枚举），顾名思义，该类型包含了全部可能的情况，可以有效地防止用户提供无效值
- 三类
  - 无参数枚举体
  - 类C枚举体
  - 携带类型参数的枚举体
    - 这样的枚举值本质上属于函数指针类型
```rust
// 无参数枚举
enum Number {
    Zero,
    One,
    Two,
}
fn test1 () {
    let a = Number::One;
    match a {
        Number::Zero => println!("0"),
        Number::One => println!("1"),
        Number::Two => println!("2"),
    }
}

// 类C枚举类型
enum Color {
    Red = 0xff0000,
    Green = 0x00ff00,
    Blue = 0x0000ff,
}
fn test2 () {
    println!("roses are #{:06x}", Color::Red as i32);
}

// 带参数枚举体
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

fn test3 () {
    let x : fn(u8, u8, u8, u8) -> IpAddr = IpAddr::V4;
    let home = IpAddr::V4(127, 0, 0, 1);
}
```
## 2.8. 常用集合类型
- 在Rust标准库std::collections模块下有4种通用集合类型
  - 线性序列：向量（Vec）、双端队列（VecDeque）、链表（LinkedList）
  - Key-Value映射表：无序哈希表（HashMap）、有序哈希表（BTreeMap）
  - 集合类型：无序集合（HashSet）、有序集合（BTreeSet）
  - 优先队列：二叉堆（BinaryHeap）

### 2.8.1. 线性序列：Vec
- 向量也是一种数组，和基本数据类型中的数组的区别在于，**向量可动态增长**
  ```rsut
  fn main() {
      let mut v1 = vec![]; // 初始化方式一
      v1.push(1);
      v1.push(2);
      v1.push(3);
      assert_eq!(v1, [1, 2, 3]);
      assert_eq!(v1[1], 2);

      let mut v2 = vec![0; 10]; // 初始化方式二
      println!("v2 is: {:?}", v2); // v2 is: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
      
      let mut v3 = Vec::new(); // 初始化方式三
      v3.push(4);
      v3.push(5);
      v3.push(6);
      // v3[4]; // error: index out of bounds
  }
  ```
- vec！是一个宏，用来创建向量字面量。
  - 宏语句可以使用圆括号，也可以使用中括号和花括号，一般使用中括号来表示数组。
- Rust对向量和数组都会做越界检查，以保证安全

### 2.8.2. 线性序列：双端队列
- Rust中的VecDeque是基于可增长的RingBuffer算法实现的双端队列
- 需要通过 use 关键字引入 std::collections::VecDeque，因为VecDeque＜T＞并不会像Vec＜T＞那样被自动引入
```rust
use std::collections::VecDeque;

fn main() {
    let mut buf = VecDeque::new();
    buf.push_front(1);
    buf.push_front(2);
    assert_eq!(buf.get(0), Some(&2));
    assert_eq!(buf.get(1), Some(&1));
    buf.push_back(3);
    buf.push_back(4);
    buf.push_back(5);
    println!("buf is:{:?}", buf); // buf is:[2, 1, 3, 4, 5]
}
```
### 2.8.3. 线性序列：链表
- Rust提供的链表是双向链表，允许在任意一端插入或弹出元素
- **通常最好使用Vec或VecDeque类型，因为它们比链表更加快速，内存访问效率更高，并且可以更好地利用CPU缓存**
- use 显示引入 std::collections::LinkedList;
```rust
use std::collections::LinkedList;
fn main() {
    let mut l1 = LinkedList::new();
    l1.push_back('a');
    let mut l2 = LinkedList::new();
    l2.push_back('b');
    l2.push_back('c');
    l1.append(&mut l2);
    println!("l1: {:?}", l1); // l1: ['a', 'b', 'c']
    println!("l2: {:?}", l2); // l2: [] 
    let a = l1.pop_front();
    match a {
        Some(x) => println!("a is: {}", x), // a is: a
        None => println!("get nill"),
    }
    println!("after pop_front: {:?}", l1); // after pop_front: ['b', 'c']
}
```
- append 的时候注意所有权的转移

### 2.8.4. Key-Value映射表：HashMap和BTreeMap
- Key必须是可哈希的类型，Value必须是在编译期已知大小的类型
- HashMap是无序的，BTreeMap是有序的
- 需要引入 包
```rust
use std::collections::BTreeMap;
use std::collections::HashMap;

fn main() {
    let mut hmap = HashMap::new();
    let mut bmap = BTreeMap::new();

    hmap.insert(3, "c");
    hmap.insert(1, "a");
    hmap.insert(2, "b");
    hmap.insert(5, "e");
    hmap.insert(4, "d");

    bmap.insert(3, "c");
    bmap.insert(1, "a");
    bmap.insert(2, "b");
    bmap.insert(5, "e");
    bmap.insert(4, "d");

    println!("hmap: {:?}", hmap); // hmap: {5: "e", 3: "c", 4: "d", 2: "b", 1: "a"}
    println!("bmap: {:?}", bmap); // bmap: {1: "a", 2: "b", 3: "c", 4: "d", 5: "e"}
}
```
### 2.8.5. 集合：HashSet和BTreeSet
HashSet＜K＞和BTreeSet＜K＞其实就是HashMap＜K，V＞和BTreeMap＜K，V＞把Value设置为空元组的特定类型，等价于`HashSet<K, ()>` 和 `BTreeSet<K，（）>`
- 集合中的元素应该是唯一的，因为是Key-Value映射表的Key
- 集合中的元素应该都是可哈希的类型
- HashSet应该是无序的，BTreeSet应该是有序

### 2.8.6. 优先队列：BinaryHeap
Rust提供的优先队列是基于二叉最大堆（Binary Heap）实现的

## 2.9. 智能指针
- Rust 中的值默认被分配到栈内存
- 可以通过 Box ＜T＞将值装箱（在堆内存中分配）；
  - Box＜T＞是指向类型为T的堆内存分配值的智能指针
  - 当Box＜T＞超出作用域范围时，将调用其析构函数，销毁内部对象，并自动释放堆中的内存
  - 可以通过解引用操作符来获取Box＜T＞中的T
  - Box＜T＞的行为像引用，并且可以自动释放内存，所以我们称其为智能指针
```rust
fn main() {
    #[derive(PartialEq, Debug)]
    struct Point {
        x: f64,
        y: f64,
    }
    let box_point = Box::new(Point {x: 0.0, y: 0.0});
    let unboxed_point: Point = *box_point;
    assert_eq!(unboxed_point, Point {x: 0.0, y: 0.0});
}
```
## 2.10. 泛型和 trait
- 泛型允许开发者编写一些在使用时才指定类型的代码
- trait是Rust实现零成本抽象的基石
  - trait是Rust唯一的接口抽象方式
  - 可以静态生成，也可以动态调用
  - 可以当作标记类型拥有某些特定行为的“标签”来使用
  - trait是对类型行为的抽象

### 2.10.1. 泛型
- 只有实现了Debug trait的类型才拥有使用＂{：？}＂格式化打印的行为

### 2.10.2. trait
```rust
struct Duck;
struct Pig;

trait Fly { // 定义 triat
    fn fly(&self) -> bool;
}
impl Fly for Duck { // 为类型实现一个 trait
    fn fly(&self) -> bool {
        return true;
    }
}
impl Fly for Pig {
    fn fly(&self) -> bool {
        return false;
    }
}
fn fly_static<T: Fly> (s: &T) -> bool { // 静态分发
    s.fly()
}
fn fly_dyn (s: &dyn Fly) -> bool { // 动态分发
    s.fly()
}
fn main() {
   let pig = Pig;
   assert_eq!(fly_static::<Pig>(&pig), false);
   assert_eq!(fly_dyn(&pig), false);
   let duck = Duck;
   assert_eq!(fly_static::<Duck>(&duck), true);
   assert_eq!(fly_dyn(&duck), true); 
}
```
- trait中也可以定义函数的默认实现
- 使用＃[derive（Debug）]属性帮助开发者自动实现Debug trait
- 静态分发
  - 在编译阶段，泛型已经被展开为具体类型的代码
- 动态分发
  - 在运行时查找相应类型的方法，会带来一定的运行时开销，不过这种开销很小

## 2.11. 错误处理
Rust 中的错误处理是通过返回 Result＜T，E＞类型的方式进行的。Result＜T，E＞类型是Option＜T＞类型的升级版本，同样定义于标准库中。
```rust
enum Result<T, E> {
  Ok(T),
  Err(E),
}
```
- 如果返回类型是 Result＜T，E＞，那么开发者就不得不处理正常和错误这两种情况，这就为程序的健壮性提供了保证。
- 在Rust 2018版本中，允许main函数返回Result＜T，E＞

## 表达式优先级
## 注释与打印
Rust文档的哲学是：代码即文档，文档即代码
- 不同的注释
  - 使用//对整行注释
  - 使用/*…*/ 对区块注释
- 文档注释 内部支持Markdown标记，也支持对文档中的示例代码进行测试，可以用rustdoc工具生成HTML文档
  - 使用///注释可以生成库文档，一般用于函数或结构体的说明，置于说明对象的上方
  - 使用//！也可以生成库文档，一般用于说明整个模块的功能，置于模块文件的头部
- 打印
  - nothing代表Display，比如println！（＂{}＂，2）
  - ？代表Debug，比如println！（＂{：？}＂，2）
  - o代表八进制，比如println！（＂{：o}＂，2）
  - x代表十六进制小写，比如println！（＂{：x}＂，2）
  - X代表十六进制大写，比如println！（＂{：X}＂，2）
  - p代表指针，比如println！（＂{：p}＂，2）
  - b代表二进制，比如println！（＂{：b}＂，2）
  - e代表指数小写，比如println！（＂{：e}＂，2）
  - E代表指数大写，比如println！（＂{：E}＂，2）