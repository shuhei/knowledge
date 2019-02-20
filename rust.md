# Rust

## Ownership

A variable's ownership goes away when:

- The variable goes out of scope (Drop)
- The variable is assigned to another variable when the type doesn't have `Copy` trait (Move)
- The variable is passed to a function as an argument when the type doesn't have `Copy` trait (Move)

(`Copy` trait doesn't work for types with `Drop`. Drop cleans up heap, but `Copy` is for types without data in heap.)
