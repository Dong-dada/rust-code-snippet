# \#\[derive\(Debug\)\]

在结构体或枚举上增加 `#[derive(Debug)]` 注解，可以在 `println!` 中打印结构体或枚举的内容：

```rust
use crate::List::{Cons, Nil};

#[derive(Debug)]
enum List {
    Cons(i32, Box<List>),
    Nil,
}

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
    
    // {:?} 输出的内容会被压缩：
    // Cons(1, Cons(2, Cons(3, Nil)))
    println!("{:?}", list);
    
    // {:#?} 输出的内容会展开：
    // Cons(
    //     1,
    //     Cons(
    //         2,
    //         Cons(
    //             3,
    //             Nil,
    //         ),
    //     ),
    // )
    println!("{:#?}", list);
}
```

