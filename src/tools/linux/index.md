# Some linux command

## `Use Stream EDitor` (SED)

### Find and replace text within a file using sed command

> sed -i 's/old-text/new-text/g' input.txt

- `-i` to update file pass
- `s` is the substitute command of sed for find and replace
- `g` means global replace i.e. find all occurrences

> sed -i 's/foo/bar/gI' input.txt

- `I` to match all cases of foo (foo, FOO, Foo, FoO) add I (capitalized I) option  