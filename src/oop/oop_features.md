# OOP Features

From the _Gang of Four_ book: Object-oriented programs are made up of objects. An object packages both data and the procedures that operate on that data. The procedures are typically called methods or operations.

## Encapsulation that Hides Implementation Details

The only way to interact with an object is through its public API. The implementation details of an object are not accessible to code using that object. For example, in rust we can define:

```rust
pub struct AveragedCollection {
    list: Vec<i32>, //private
    average: f64, //private
}
```

```rust
impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }

    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            }
            None => None,
        }
    }

    pub fn average(&self) -> f64 {
        self.average
    }

    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

We can change the data structure from a vector to a hashmap without breaking the public API.

## Inheritance

Two reasons to use inheritance:

1. Reuse of code
2. Polymorphism: make use of the type system. To enable a child type to be used in the same places as the parent type. _Polymorphism_ means you can substitute multiple objects for each other at runtime if they share certain characteristics.

Inheritance has recently fallen out of favor as a programming design solution in many programming languages because it's often at risk of sharing more code than necessary. It is less flexible that a subclass share all characteristics of their parent class.
