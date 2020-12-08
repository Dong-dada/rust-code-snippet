# Rc&lt;T&gt;

基本使用：

```rust
use crate::List::{Cons, Nil};
use std::rc::Rc;

enum List {
    Cons(i32, Rc<List>),
    Nil
}

fn main() {
    // 使用 Rc<T>::new() 方法来创建一个 Rc 指针，使用 Rc::strong_count() 来获取当前的引用计数
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    println!("count after creating a = {}", Rc::strong_count(&a));

    // 使用 Rc::clone() 方法来复制一个 Rc 指针，此时 a, b 都指向同样的内容
    let b = Cons(3, Rc::clone(&a));
    println!("count after creating b = {}", Rc::strong_count(&a));

    {
        let c = Cons(4, Rc::clone(&a));
        println!("count after creating c = {}", Rc::strong_count(&a));
    }
    println!("count after c goes out of scope = {}", Rc::strong_count(&a));

    // 输出以下内容:
    // count after creating a = 1
    // count after creating b = 2
    // count after creating c = 3
    // count after c goes out of scope = 2
}
```

