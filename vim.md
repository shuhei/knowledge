# Vim

## Regular Expression

`Array<Foo>` -> `Foo[]`:

```
:%s/Array<\([^>]*\)>/\1[]/g
```

- Some of the symbols that have special meanings in regular expression have to be escaped
  - Escape: `()` -> `\(\)`, `{n}` -> `\{n}` (only one `\`!), `+` -> `\+`
  - No escape: `[]`, `*`
- Backreferences are available with `\1`, `\2`, and so on
