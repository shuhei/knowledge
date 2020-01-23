# Java

## Data Types

### int, Integer

- `Integer -> int`: `integer.intValue()`
- `Integer`: Use `equals()` for equality check. Use `compareTo()` or `Integer.compare()` instead of `<=` or `>=`.

### char, Character

- [`char` has 16 bits](https://docs.oracle.com/javase/tutorial/i18n/text/unicode.html) to represent a UTF-16 character
- `char '0' - '9'` to `int` digit: `int digit = c - '0';` or `int digit = Character.digit(c, 10);`
- `char 'a' - 'z'` to `int` index: `int index = c - 'a';`
  - Useful for using an array of alphabets `new T[26]` instead of `new HashMap<Character, T>()`

### String

- `str.length()` for the length (not `.length` like an array!)
- `str.charAt(index)` to get the `char` at the index

### Collections

- List:
  - Get an immutable empty list: `Collections.emptyList()`
  - An array to a list: `new ArrayList(Arrays.asList(array))`
  - A primitive array to a list:
    ```java
    List<Integer> list = new ArrayList<>();
    for (int n : nums) {
      list.add(n);
    }
    ```
- Stack: Use `ArrayDeque`
- Heap: Use `PriorityQueue` (min heap). For a max heap, `new PriorityQueue<>(Collections.reverseOrder())`
  - Min heap is useful for finding `k` largest items from a large list. Max heap is useful for finding `k` smallest items.
- Double-ended queue: `ArrayDeque` or `LinkedList` https://stackoverflow.com/questions/6163166/why-is-arraydeque-better-than-linkedlist
  - A queue can be implemented with two stacks or a dynamically resizin ring buffer (both `O(n)` amortized time complexity)
- Set
  - `contains(value)` to check if it has the given value
  - `add(value)` to add a value
  - An array to `Set`: `new HashSet<>(Arrays.asList(array))`
- Map
  - `containsKey(key)` to check if it has the given key
  - `put(key, value)` to add an entry
  - To loop through entries:
    ```java
    for (Map.Entry<K, V> entry : map.entrySet()) {
      K key = entry.getKey();
      V value = entry.getValue();
    }
    // or
    map.forEach((k, v) -> {
      // Do something
    });
    ```
  - `LinkedHashMap` for maintaining insertion order or access order of the entries
    - Insertion order: default
      - Insertion order is **not** updated by re-insertion.
    - Access order:
      - `new LinkedHashMap(initialCapacity, loadFactor, true)`
      - Access order is **not** updated by `containsKey`.
      - Access order is updated by `get()`, `put()`, `compute()` and their friends.

### Streams

```java
List<T> list = someCollection.stream()
  .map(x -> x + x) // do something
  .collect(Collectors.toList());
```

## Syntax

### Access Levels

- A class can be:
  - package-private (default)
  - public
- A class member can be:
  - private
  - package-private (default)
  - protected (package-private + accessible to subclasses)
    - If the class itself is public, protected members are part of the public API because users can depend on them
  - public

### Nested classes

- static member class
- non-static member class
  - A non-static member has an implicit reference to its enclosing class instance and has access to its members. Use static member class if possible because the reference costs time and space, and may cause memory leak.
- anonymous class
- local class

### final

- A final class can't be subclassed.
- A final method can't be overridden by subclasses.
- A final variable/member can't be reassigned. However, it doesn't stop mutable objects from being mutated.

## Concurrency

- `volatile` on a variable prevents the variable from being cached in a thread (puts the variable in the main memory instead of CPU cache)
- `synchronized` on a method (or a code block) takes a lock of the instance (or a given object) while executing it

Tutorials

- [Lesson: Concurrency - The Java Tutorials](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Java Concurrency in Practice](http://jcip.net/)
- [Java Concurrency and Multithreading Tutorial](http://tutorials.jenkov.com/java-concurrency/index.html)
- [Java Multithreading - Udemy](https://www.udemy.com/java-multithreading/)
