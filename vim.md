# Vim

## Regular Expression

- Some of the symbols that have special meanings in regular expression have to be escaped
  - Escape: `()` -> `\(\)`, `{n}` -> `\{n}` (only one `\`!), `+` -> `\+`
  - No escape: `[]`, `*`
- Backreferences are available with `\1`, `\2`, and so on

### Examples

`Array<Foo>` -> `Foo[]`:

```vim
:%s/Array<\([^>]*\)>/\1[]/g
```

## How plugins work

- http://learnvimscriptthehardway.stevelosh.com/chapters/42.html
- `:help plugins`

### Check which plugins are loaded

```vim
:scriptnames
```

[How do I list loaded plugins in Vim?](https://stackoverflow.com/questions/48933/how-do-i-list-loaded-plugins-in-vim)

## Tips

### Transform the current buffer with a command

```vim
:%! <command>
```

For example:

```vim
:%! prettier
:%! sort
```

https://vi.stackexchange.com/questions/5835/how-to-run-bash-command-over-current-file-and-replace-buffer-with-result

### Turn off auto-formatting

I have several plugins that try to format buffers on save. coc.nvim, vim-better-whitespace, editorconfig, oh my! Forget about them and just run:

```vim
:noa w
```

https://stackoverflow.com/questions/41070645/prevent-vims-autocmd-from-running-on-write-just-once
