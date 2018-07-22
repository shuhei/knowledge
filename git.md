# Git

## `git grep`

- To use [lookahead/lookbehind](https://www.regular-expressions.info/lookaround.html) in regex, install `git` with PCRE support and use `git grep -P`
  - On Mac, `brew reinstall --with-pcre2 git`
  - Example of negative lookbehind:  `'(?<!this\.)foo'` matches `foo` except `this.foo`
