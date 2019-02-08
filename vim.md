# Vim

## Regular Expression

`Array<Foo>` -> `Foo[]`:

```
:%s/Array<\([^>]*\)>/\1[]/g
```

- `()`for grouping needs to be escaped as `\(\)`
- Backreferences are available with `\1`, `\2`, and so on
