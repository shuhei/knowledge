# Java

## Data Types

### int, Integer

- `Integer -> int`: `integer.intValue()`

### char, Character

- `char '0' - '9'` to `int` digit: `int digit = c - '0';` or `int digit = Character.digit(c, 10);`
- `char 'a' - 'z'` to `int` index: `int index = c - 'a';`
  - Useful for using an array of alphabets `new T[26]` instead of `new HashMap<Character, T>()`

### String

- `str.length()` for the length (not `.length` like an array!)
- `str.charAt(index)` to get the `char` at the index

## Concurrency

- `volatile` on a variable prevents the variable from being cached in a thread (puts the variable in the main memory instead of CPU cache)
- `synchronized` on a method (or a code block) takes a lock of the instance (or a given object) while executing it

Tutorials

- [Lesson: Concurrency - The Java Tutorials](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Java Concurrency in Practice](http://jcip.net/)
- [Java Concurrency and Multithreading Tutorial](http://tutorials.jenkov.com/java-concurrency/index.html)
- [Java Multithreading - Udemy](https://www.udemy.com/java-multithreading/)
