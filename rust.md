# Rust

## Learning Materials

- [Rust Book](https://doc.rust-lang.org/book/)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rustlings](https://github.com/rust-lang/rustlings)

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

## Option/Result

- `or`: provides a default value `Option<T>`/`Result<T, F>`
  - It can change the error type of `Result`
- `or_else`: provides a default value with a function `() -> Option<T>`/`(E) -> Result<T, F>`
  - It can change the error type of `Result`
- `and_then`: `>>=` of Haskell, `andThen` of Elm, `flatMap` of Scala
- `and`: does `and_then` with a value instead of a function
- `map_or`: does `map` and `or` at once
- `map_or_else`: does `map` and `or_else` at once
- `ok_or`: converts a `Option<T>` to `Result<T, E>` with a provided default value
- `ok_or_else`: does `ok_or` with a function
