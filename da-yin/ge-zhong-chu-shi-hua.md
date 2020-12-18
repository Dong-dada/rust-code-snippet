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

