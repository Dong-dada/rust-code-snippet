# 结构体 - 基本

基本使用:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

// 为结构体添加 method
impl Rectangle {
    // 使用 &self, 以只读方式访问 Rectangle 实例
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // 使用 &mut self, 以读写方式访问 Rectangle 实例
    fn scale(&mut self, radio: f32) {
        self.width = (self.width as f32 * radio) as u32;
        self.height = (self.height as f32 * radio) as u32;
    }
}

// 注意同一个结构体，可以定义多个 impl block.
impl Rectanble {
    // 定义 associated function, 类似于静态成员函数
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let rect = Rectangle {
        width: 30,
        height: 50,
    };

    // 调用成员函数
    println!("The area of the rectangle is {} square pixels.", rect.area());

    // 调用成员函数修改结构体成员
    let mut rect = rect;
    rect.scale(0.5);
    println!("{:#?}", rect);

    // 调用 associated function
    let rect = Rectangle::square(3);
    println!("{:#?}", rect);
}
```

Tuple struct:

```rust
fn main() {
    // tuple struct, 类似于 tuple, 但是可以起名字
    struct Color(i32, i32, i32);
    struct Point(i32, i32, i32);

    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

