# Drop Trait

Drop Trait 类似于 C++ 的析构函数，让你可以在指针及其内容被析构前做一些事情。

```rust
struct CustomSmartPointer {
    data: String,
}

// Drop trait 类似于 C++ 里的析构函数，你可以在指针及其内容被析构时做一些事情
impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        // 打一行日志记录对象析构
        println!("Dropping CustomSmartPointer with data `{}`", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer {
        data: String::from("my stuff"),
    };
    let d = CustomSmartPointer {
        data: String::from("other stuff"),
    };
    println!("CustomSmartPointers created.");

    // 使用 std::mem::drop() 方法可以提前析构一个对象
    // 注意 drop(c) 会夺取所有权，所以调用了 drop(c) 之后就不能再访问这个变量了。
    std::mem::drop(c);
    println!("CustomSmartPointers dropped the end of main.")

    // 输出结果为:
    // CustomSmartPointers created.
    // Dropping CustomSmartPointer with data `my stuff`
    // CustomSmartPointers dropped the end of main.
    // Dropping CustomSmartPointer with data `other stuff`
}
```

