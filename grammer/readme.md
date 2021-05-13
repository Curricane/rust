# 简介
用于 rust 语法的学习
## 初识 rust
- Rust 不支持 ++ 和 --，但支持 += 
- 64.0 将表示 64 位浮点数
- 数字间可以用 _ 连接， 98_222
- 注释方式与 golang c java 一样
## 变量
1. Rust 是强类型语言，但具有自动判断变量类型的能力
2. 如果要声明变量，需要使用 let 关键字
    1. `let a = 123;`
    2. let [name] : [type] 比如：`let a: i32;`
3. Rust 语言不允许精度有损失的自动数据类型转换
4. 变量的值可以"重新绑定"，但在"重新绑定"以前不能私自被改变，这样可以确保在每一次"绑定"之后的区域里编译器可以充分的推理程序逻辑
### 不可变变量
Rust 语言为了高并发安全而做的设计：在语言层面尽量少的让变量的值可以改变  
如果我们编写的程序的一部分在假设值永远不会改变的情况下运行，而我们代码的另一部分在改变该值，那么代码的第一部分可能就不会按照设计的意图去运转  
默认声明的变量是不可变的
```rust
let a = 123; // 没有声明类型，自动判断为有符号的32位整型变量
// a = 456; // 不合法
let a = 45; //合法 其实用到了【重影】

const a: i32 = 123; // a 是常量
let a = 456; // 不合法
```
### 可变变量
```rust
let mut a = 123;
a = 456;
```
### 重影
重影就是刚才讲述的所谓"重新绑定"，是指变量的名称可以被重新使用的机制。
1. 用同一个名字重新代表另一个变量实体，其类型、可变属性和值都可以变化
2. 与可变变量赋值的区别，可变变量赋值仅能发生值的变化  
```rust
fn main() {
    let x = 5;
    let x = "23333";
    println!("x is {}", x);
}
```
## 数据类型
### 基本数据类型
所有整数类型，例如 i32 、 u32 、 i64 等。  
布尔类型 bool，值为 true 或 false 。  
所有浮点类型，f32 和 f64。  
字符类型 char。  
仅包含以上类型数据的元组（Tuples）。  
### 布尔
- 布尔型用 bool 表示，值只能为 true 或 false
### 字符
- Rust 中字符串和字符都必须使用 UTF-8 编码，否则编译器会报错
- Rust的 char 类型大小为 4 个字节，代表 Unicode标量值
### 元组
元组用一对 ( ) 包括的一组数据，**可以包含不同种类的数据**：
```rust
let  tup: (i32, f64, u8) = (500, 6.4, 1);
// tup.0 == 500
// tup.1 == 6.4
// tup.2 == 1
let (x, y, z) = tup; // y == 6.4
```
### 数组
[ ] 包括的同类型数据
```rust
let a = [1, 2, 3, 4, 5];
let b = ["Jan", "Feb", "Mar"];
let c: [i32; 5] = [1, 2, 3, 4, 5];
let d = [3; 5] // equal to let d = [3, 3, 3, 3, 3];

let first = a[0]
let second = a[1]
// a[0] = 123; // error. 数组 a 不可变
let mut f = [1, 2, 3];
f[0] = 4; // 正确
```
## 函数
- `fn <name> (<params>) <函数体>`
- Rust 函数的命名风格是小写字母以下划线分割； `hello_world`
- 需要具备参数必须声明参数名称和类型
- 函数体的语句和表达式
    - 语句 Statement 是执行某些操作且**没有返回值**的步骤，往往以`;`结尾，如`let a = 6;`
    - 表达式 Expression 有计算不走且**有返回值**，不以`;`结尾，如`a = 7`  `b + 2` `c * (a + b)`
    - 表达式 加上`;`后就是语句
    - 可以用`{}`包括的块里编写一个较为复杂的表达式，其中 最后一行需要为表达式，而不是语句，即最后一行不能加`;`
        ```rust
        fn main() {
            let x = 5;
            let y = {
                let x = 3;
                x + 1 // 没有 ; 号结尾，是表达式
            }; // ; 号之前是一个复杂的表达式，加上 ; 是一个复杂的语句

            println!("x: {}", x); // 5
            println!("y: {}", y); // 4
        }
        ```
- 函数返回值
    - 使用`->`指明返回类型
    - Rust 不支持自动返回值类型判断！如果没有明确声明函数返回值的类型，函数将被认为是"纯过程"，不允许产生返回值
    - return 后面不能有返回值表达式
    - 可以不用return返回，而用 表达式返回
    ```rust
    fn add(a: i32, b: i32) -> i32 {
        return a + b;
    } 

    fn del(a: i32, b: i32) -> i32 {
        a - b
    }
    ```
- 函数定义可以嵌套
    ```rust
    fn main() {
        fn five() -> i32 {
            5
        }
        println!("five() 的值为：{}", five());
    }
    ```
## 条件语句
- 与  golang 类似，且同样条件不需要 `()`括起来
- 条件 必须是 布尔类型，不用通过判空、判非0，来代替true or false
```rust
fn main() {
    let a = 12;
    let b;
    let a > 0 {
        b = 1;
    }
    else if a < 0 {
        b = -1;
    }
    else {
        b = 0;
    }
    println!("b is {}", b);
}
```
### 三元运算
```rust
fn main() {
    let a = 3;
    let number = if a > 0 {1} else {-1};
    println("number is {}", number);
}
```
## 循环
- while
    ```rust
    fn main() {
        let mut n = 1;
        while n != 4 {
            println!("{}", n);
            n += 1;
        }
        println!("EXIT");
    }
    ```
- for in
    ```rust
    fn main() {
        let a = [1,2,3,4,5];
        for i in a.iter() {
            println!("i is {}", i);
        }

        for i in 0..3 { // 0 1 2
            println!("a[{}] = {}", i, a[i]);
        }
    }
    ```
- loop 
    - 无限循环
    - 通过 break 关键字类似于 return 一样使整个循环退出并给予外部一个返回值。
    ```rust
    fn main() {
        let s = ['R', 'U', 'N', 'O', 'O', 'B'];
        let mut i = 0;
        loop {
            let ch = s[i];
            if ch == 'O' {
                break;
            }
            println!("\'{}\'", ch);
            i += 1;
        }

        let location = loop {
            let ch = s[i];
            if ch == 'O' {
                break i; // 跳出循环，并返回一个值
            } 
            i += 1;
        };
    }
    ```
## 所有权
-  三大规则
    - Rust 中的每个值都有一个变量，称为其所有者
    - 一次只能有一个所有者
    - 当所有者不在程序运行范围时，该值将被删除
### 内存和分配
Rust 之所以没有明示释放的步骤是因为在变量范围结束的时候，Rust 编译器自动添加了调用释放资源函数的步骤。
### 变量数与数据交互的方式
- 移动 Move
    - 仅在栈中的数据的移动方式是直接复制
        ```rust
        let x = 5;
        let y = x; 
        // 此时 栈中有两个 5
        ```
    - 堆中的数据的移动方式，是把所有权移动给移动，如A移动给B，则移动后A没有所有权，失效了，不能再使用了
        ```rust
        let s1 = String::from("hello");
        let s2 = s1; // 此时 s1 把字符串 hello 的所有权给了 s2，自己却失去了所有权
        println!("{}, world", s1); // error s1 已经失效，名存实亡
        ```
        ![堆上所有权移动](./picture/堆上所有权移动.png)
- 克隆 Clone
    - 将数据单纯的复制一份以供他用
    ```rust
    fn main() {
        let s1 = String::from("hello");
        let s2 = s1.clone();
        println!("s1 = {}, s2 = {}", s1, s2);
    }
    ```
### 涉及函数的所有权机制
- 如果把变量当做参数传入函数，那么它和移动的效果是一样的
- 函数返回值的所有权机制
```rust 
fn main() {
    let s1 = gives_ownership();
    // gives_ownership 移动它的返回值到 s1

    let s2 = String::from("hello");
    // s2 被声明有效

    let s3 = takes_and_gives_back(s2);
    // s2 被当作参数移动, s3 获得返回值所有权
} // s3 无效被释放, s2 被移动, s1 无效被释放.

fn gives_ownership() -> String {
    let some_string = String::from("hello");
    // some_string 被声明有效

    return some_string;
    // some_string 被当作返回值移动出函数
}

fn takes_and_gives_back(a_string: String) -> String { 
    // a_string 被声明有效

    a_string  // a_string 被当作返回值移出函数
}
```
### 引用与租借
引用（Referrence）可以理解为变量的一个受限的别名，类似c++中的引用，引用就是租借引用对象的所有权.  
当一个变量的值被引用时，变量本身不会被认定无效。因为"引用"并没有在栈中复制变量的值：  
![引用](./picture/引用.png)
- 引用的方式:  
    - 不可变的引用 `let s2 = &s1;`
    - 可变的引用 `let s2 = &mut s1;`
- 需要注意的地方
    - 当被引用的对象失去值的所有权，引用也失效
    - 同时只能被租借一次
    - 被租借后，租借失效之前，不能被移动
    - 可以同时有多个读租借，同时只能有一个写租借
#### 垂悬引用（Dangling References）
"垂悬引用"在 Rust 语言里不允许出现，如果有，编译器会发现它  
```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
}
```