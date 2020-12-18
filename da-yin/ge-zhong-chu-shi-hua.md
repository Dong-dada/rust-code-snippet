# 各种初始化

## 初始化 HashSet, HashMap

使用 maplit crate 可以方便地对 HashSet, HashMap 进行初始化：

```rust
#[macro_use] extern crate maplit;

use std::collections::{HashSet, HashMap};

fn main() {
    // 使用 maplit 库中的 hashset! 宏来初始化 HashSet
    let number_set: HashSet<u8> = hashset!{1, 2, 3, 4, 5, 6, 7, 8, 9};
    println!("{:?}", number_set);

    // maplit 库当中还有 hashmap! 宏，可以用来方便地初始化 HashMap
    let number_map: HashMap<u8, char> = hashmap! {
        0 => '0',
        1 => '1',
        2 => '2',
    };
    println!("{:?}", number_map);
}
```

## 初始化结构体数组

```rust
#[derive(Debug)]
struct Slot {
    number: u8,
    modifiable: bool,
}

// 实现 Default trait, 这样可以调用 Default::default() 来构造默认值
impl Default for Slot {
    fn default() -> Slot {
        Slot {
            number: 0,
            modifiable: true,
        }
    }
}

fn main() {
    let slots: [Slot; 9] = Default::default();
    println!("{:?}", slots);
}
```

注意以下写法要求结构体必须实现 Copy trait，因此无法编译通过：

```rust
#[derive(Debug)]
struct Slot {
    number: u8,
    modifiable: bool,
}

fn main() {
    // 编译报错: the trait `std::marker::Copy` is not implemented for `Slot`
    let slots = [Slot{number: 0, modifiable: true}; 9];
    println!("{:?}", slots);
}
```

