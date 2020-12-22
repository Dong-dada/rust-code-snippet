# 观察者模式



```rust
use std::rc::{Weak, Rc};
use std::cell::RefCell;

#[derive(Debug, Copy, Clone, PartialEq)]
pub enum CounterEvent {
    Start,
    TenRemaining,
    FiveRemaining,
    Finish,
}

pub trait CounterEventObserver {
    // 因为 Observer 实现者在收到事件后，往往会更新自己，因此需要将 self 设置为 &mut
    fn on_event(&mut self, event: &CounterEvent);
}

pub struct Counter {
    target_count: u32,
    current_count: u32,

    // 使用 Weak<dyn T> 来保存 observer, 避免循环引用导致内存泄漏
    // 为了能够在调用 observer.on_event() 时传入 &mut self, 需要使用 RefCell 来包装一下
    observers: Vec<Weak<RefCell<dyn CounterEventObserver>>>,
}

impl Counter {
    pub fn new(target_count: u32) -> Counter {
        Counter {
            target_count,
            current_count: 0,
            observers: vec![]
        }
    }

    // 添加观察者
    pub fn add_observer(&mut self, observer: Rc<RefCell<dyn CounterEventObserver>>) {
        let observer = Rc::downgrade(&observer);
        self.observers.push(observer);
    }

    // 增加计数器，随着计数的增加，将触发事件通知给观察者
    pub fn plus_one(&mut self) {
        if self.current_count == self.target_count {
            return;
        }
        self.current_count += 1;

        if self.target_count - self.current_count == 10 {
            self.fire_event(&CounterEvent::TenRemaining);
        } else if self.target_count - self.current_count == 5 {
            self.fire_event(&CounterEvent::FiveRemaining);
        } else if self.target_count == self.current_count {
            self.fire_event(&CounterEvent::Finish);
        }
    }

    // 私有函数，用来触发事件
    fn fire_event(&mut self, event: &CounterEvent) {
        self.observers.retain(|observer| {
            // 先尝试将 Weak<RefCell<T>> 提权为 Rc<RefCell<T>>
            if let Some(observer) = observer.upgrade() {
                // 然后通过 RefCell::borrow_mut() 来获取可变引用
                observer.borrow_mut().on_event(event);
                return true;
            }

            // 提权失败，返回 false 就能移除这个 observer
            return false;
        });
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    struct MockObserver {
        current_event: CounterEvent,
    }

    impl CounterEventObserver for MockObserver {
        fn on_event(&mut self, event: &CounterEvent) {
            println!("receive event: {:?}", event);
            self.current_event = *event;
        }
    }

    #[test]
    fn observer_exists() {
        let mut counter = Counter::new(12);

        let observer = MockObserver{ current_event: CounterEvent::Start };
        let observer = Rc::new(RefCell::new(observer));
        counter.add_observer(observer.clone());

        for _ in 0..12 {
            counter.plus_one();
        }

        assert_eq!(observer.borrow().current_event, CounterEvent::Finish);

        // 输出结果:
        // receive event: TenRemaining
        // receive event: FiveRemaining
        // receive event: Finish
    }

    #[test]
    fn observer_not_exists() {
        let mut counter = Counter::new(12);

        {
            let observer = MockObserver{ current_event: CounterEvent::Start };
            let observer = Rc::new(RefCell::new(observer));
            counter.add_observer(observer.clone());
        }   // observer was dropped

        for _ in 0..12 {
            counter.plus_one();
        }

        // observer should be removed
        assert_eq!(counter.observers.len(), 0);
    }
}

```

