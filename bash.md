# Bash

## IO Redirection

https://www.brianstorti.com/understanding-shell-script-idiom-redirect/

```sh
echo hello > hello.txt
# is same as:
echo hello 1> hello.txt
```

`n>` means redirecting the file descriptor `n` to a file. When `n` is not given, `1` (stdout) is used by default.

```sh
some_command > /dev/null 2>&1
# is same as:
some_command 1> /dev/null 2> /dev/null
```

`m>&n` means redirecting the file descriptor `m` to the same file that the file descriptor `n` redirects to. In this case, `2>&1` means that `2` (stderr) redirects to `/dev/null`, which `1` redirects to.

## Ignore an error with `set -e` while keeping the exit status

```sh
set -e

SOME_EXIT_STATUS
do_something || SOME_EXIT_STATUS=$?

# ... do something else

if [ $SOME_EXIT_STATUS -ne 0 ]; then
  exit $SOME_EXIT_STATUS
fi
```
