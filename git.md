# Git

## `git grep`

- To use [lookahead/lookbehind](https://www.regular-expressions.info/lookaround.html) in regex, install `git` with PCRE support and use `git grep -P`
  - On Mac, `brew reinstall --with-pcre2 git`
  - Example of negative lookbehind:  `git grep '(?<!this\.)foo'` matches `foo` except `this.foo`

## Clean up local braches that were squash-merged on GitHub

https://medium.com/opendoor-labs/cleaning-up-branches-with-githubs-squash-merge-43138cc7585e
