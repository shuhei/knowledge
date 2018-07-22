# Unix/Linux Commands

## Replace string in multiple files

```sh
git grep --name-only ${from} | xargs perl -0777 -i -pe "s/${from}/${to}/g"

# Multiple line pattern
git grep --name-only ${from1} | xargs perl -0777 -i -pe "s/${from1}\n${from2}/${to}/gm"
```
