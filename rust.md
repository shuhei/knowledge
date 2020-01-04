# Rust

## Learning Materials

- [Rust Book](https://doc.rust-lang.org/book/)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rustlings](https://github.com/rust-lang/rustlings)
- [Learning Rust With Entirely Too Many Linked Lists](http://cglab.ca/~abeinges/blah/too-many-lists/book/README.html)

## Ownership

A variable's ownership goes away when:

- The variable goes out of scope (Drop)
- The variable is assigned to another variable when the type doesn't have `Copy` trait (Move)
- The variable is passed to a function as an argument when the type doesn't have `Copy` trait (Move)

(`Copy` trait doesn't work for types with `Drop`. Drop cleans up heap, but `Copy` is for types without data in heap.)

### References and Borrowing

Borrowing makes a Reference.

- Immutalbe reference (`&`): References are immutable by default
- Mutable reference (`&mut`): A mutable reference cannot co-exist with other references (immutable or mutable)

### Slices

- A slice is a reference to a part of array, string, etc.
- String literals are slices (`&str`), which are immutable references to the binary's data region

### Tips

#### Swap fields in a struct

```rs
struct Something {
  prev: Foo;
  cur: Foo;
}

impl Something {
  fn set_foo(&self, foo: Foo) {
    self.prev = self.cur; // <-- cannot move out of borrowed content
    self.cur = foo;

    // Use std::mem instead!
    self.prev = std::mem::replace(&mut self.last, foo);
  }
}
```

https://stackoverflow.com/questions/35649968/how-to-swap-two-fields-of-a-struct

## Option/Result

Methods that keep wrapping (`Option -> Option`/`Result -> Result`):

- `or`: provides a default value `Option<T>`/`Result<T, F>` keeping `Option/Result`
  - Similar to `withDefault` of Elm, but `or` can change the error type of `Result`
- `or_else`: provides a default value with a function `() -> Option<T>`/`(E) -> Result<T, F>` keeping `Option/Result`
  - It can change the error type of `Result`
- `and_then`: `>>=` of Haskell, `andThen` of Elm, `flatMap` of Scala
- `and`: does `and_then` with a value instead of a function
- `map_or`: does `map` and `or` at once
- `map_or_else`: does `map` and `or_else` at once

Unwrapping methods:

- `unwrap`: unwraps the value of `Some`/`Ok` and panics for `None`/`Err`
- `unwrap_or`: `unwrap` but returns a provided default value for `None`/`Err` instead of panicking
  - `withDefault` of Elm
- `unwrap_or_else`: `unwrap_or` with a function
- `unwrap_or_default`: `unwrap_or` but with the default value from `Default` trait
- `unwrap_err` (only for `Result`): unwraps the error of `Err` and panics for `Ok`

Conversion (only for `Option`):

- `ok_or`: converts a `Option<T>` to `Result<T, E>` with a provided default value
- `ok_or_else`: does `ok_or` with a function

## Error Handling

Use [failure crate](https://github.com/rust-lang-nursery/failure), especially https://rust-lang-nursery.github.io/failure/error-errorkind.html.

For explicit early return, use `Err(ErrorKind::Foo)?;` instead of `return Err(ErrorKind::Foo);` (compile error) because `?` applies conversion with `from()`.

## Modules

https://doc.rust-lang.org/book/ch07-02-modules-and-use-to-control-scope-and-privacy.html

- Use `use crate::foo;` instead of `use self::foo;` to make it easier to move files around
- It's idiomatic to import:

  - function's parent module for functions

    ```rs
    use foo::bar;

    bar::baz();
    ```

  - enums, struct, etc. directly

## Traits

- `PartialEq` vs `Eq`
  - `PaftialEq`
    - symmetric `a == b -> b == a`
    - transitive `a == b and b == c -> a == c`
    - But doesn't need to be reflexive `a == a` (For example, `NaN != NaN` in floating point numbers)
  - `Eq`: `PartialEq` + reflexive `a == a`
- `PartialOrd` vs `Ord`
  - `PartialOrd`:
    - antisymmetric `a < b -> !(a > b)` `a > b -> !(a < b)`
    - transitive `a < b and b < c -> a < c`
    - But doesn't need to be total (For example, `NaN < 0 == false` and `NaN >= 0 == false` for floating point numbers)
  - `Ord`: `PatialOrd` + total (exactly one of `a < b`, `a == b`, `a > b` is true)
- `Iterator` vs `IntoIterator`
  - `Iterator` keeps a pointer to the current item and is inherently mutable. `Vec<T>` is **not** an `Iterator`.
  - `IntoIterator` can create a `Iterator`. Useful for `for .. in` loop. `Vec<T>` is an `IntoIterator`.

## Cargo

- Make it easier to manage project dependencies: Install [cargo-edit](https://github.com/killercup/cargo-edit)
- `extern crate foo` is not necessary in the 2018 edition
