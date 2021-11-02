# 语法面观
## 模式匹配
- 模式匹配是一种结构性的结构
    ```rust
    fn main() {
        let (a, b) = (1, 2);
        let Point{ x, y } = Point {x:3, y:4};
    }
    ```
- Rust 中支持模式匹配的位置
    - let 声明
    - 函数和闭包函数
    - match 表达式
    - if let 表达式
    - while let 表达式
    - for 表达式
```rust
fn main() {
    let x: &Option<i32> = &Some(3);

    // 当一个引用值与一个非引用值匹配的时候，编译器就会自动
    // 填充 ref 或者 ref mut; 为了提升开发体验
    // x 为引用类型，Some(y) 是非引用，则会自动填充 ref 为 Some(ref y)
    if let Some(y) = x {
        // 这里得到 &i32 类型
        y;
    }
    println!("Hello, world!");
}
```
## 智能指针
- 使用 *x 这样手动解引用的方式，等价于 `*(x.deref())`
- 使用`.`调用或在函数参数位置上对 x 自动解引用，则是等价于 `x.deref()`
## 字符与字符串
### 字符
- 码位数用`String::from("❤️").chars().count()`就可以了