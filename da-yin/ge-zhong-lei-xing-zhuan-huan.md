# 各种类型转换

## &str &lt;=&gt; Vec&lt;char&gt;

```rust
fn main() {
    // &str => Chars => Vec<char>
    let str = "Aloha!";
    let char_vec: Vec<char> = str.chars().collect();
    println!("{:?}", char_vec);

    // Vec<char> => String => &str
    let char_vec = vec!['H', 'e', 'l', 'l', 'o', '!'];
    let str: String = char_vec.into_iter().collect();
    let str = str.as_str();
    println!("{}", str);

    // 输出结果
    // ['A', 'l', 'o', 'h', 'a', '!']
    // Hello!
}
```

## 0u8 &lt;=&gt; '0'

也就是把 u8 类型的数字 \[0 - 9\] 转换为 char 类型的 \['0' - '9'\]，或者反之。

```rust
use std::convert::TryFrom;

fn main() {
    // 将 u8 类型的数字，转换为相应的 char
    let number: u8 = 0;
    let number_char = char::from(number + 48);
    println!("{}", number_char);

    // 将 char 转换为 u8 类型的数字
    let number_char = '9';
    let number = u8::try_from(number_char.to_digit(10).unwrap()).unwrap();
    println!("{}", number);

    // 输出结果:
    // 0
    // 9
}
```

也可以使用 char::from\_digit 方法来把 u8 转为 char，但是这个方法目前没有在 stable 版本中：

```rust
fn main() {
    let number: u8 = 0;
    let number_char = char::from_digit(number as u32, 10).unwrap();
    println!("{}", number_char);

    // 输出结果:
    // 0
}
```

## 使用数组初始化 HashSet 

```rust
use std::collections::HashSet;

fn main() {
    // 使用数组来初始化 HashSet
    // iter() 返回的是 &T 类型的元素，使用 cloned() 可以让迭代器返回的类型变为 T
    let number_set: HashSet<u8> = [1, 2, 3, 4, 5, 6, 7, 8, 9].iter().cloned().collect();
    println!("{:?}", number_set);

    // 使用 Vec 来初始化 HashSet
    // 初始化完成后，原先的数组就不能用了，因为使用了 into_iter(), 它会把数组的元素 move 到 HashSet 里面。
    let number_vec: Vec<u8> = vec![1, 2, 3, 4, 5, 6, 7, 8, 9];
    let number_set: HashSet<u8> = number_vec.into_iter().collect();
    println!("{:?}", number_set);

    // 使用 Vec 来初始化 HashSet
    // 初始化完成后，原先的数组还能使用，因为进行了 clone
    let number_vec: Vec<u8> = vec![1, 2, 3, 4, 5, 6, 7, 8, 9];
    let number_set: HashSet<u8> = number_vec.iter().cloned().collect();
    println!("{:?}", number_set);
    println!("{:?}", number_vec);

    // 输出结果:
    // {8, 7, 4, 9, 6, 3, 5, 1, 2}
    // {3, 8, 2, 5, 4, 7, 1, 6, 9}
    // {9, 3, 2, 5, 4, 1, 7, 6, 8}
    // [1, 2, 3, 4, 5, 6, 7, 8, 9]
}
```

