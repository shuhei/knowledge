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
        ```
        for (Map.Entry<K, V> entry : map.entrySet()) {
          K key = entry.getKey();
          V value = entry.getValue();
        }
        ```

## Concurrency

- `volatile` on a variable prevents the variable from being cached in a thread (puts the variable in the main memory instead of CPU cache)
- `synchronized` on a method (or a code block) takes a lock of the instance (or a given object) while executing it

Tutorials

- [Lesson: Concurrency - The Java Tutorials](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Java Concurrency in Practice](http://jcip.net/)
- [Java Concurrency and Multithreading Tutorial](http://tutorials.jenkov.com/java-concurrency/index.html)
- [Java Multithreading - Udemy](https://www.udemy.com/java-multithreading/)
