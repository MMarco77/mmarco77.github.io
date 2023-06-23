# Bash tips

## touch file into sub unexisting folder

```shell
$ file="./nested/folder/deep/more.txt"
$ mkdir -p "${file%/*}" && touch "$file"
```

Where

```shell
$ file="./nested/folder/deep/more.txt"
$ echo "${file%/*}" # dirname ${file}
./nested/folder/deep
$ echo "$file"
```

Possible function: 

```shell
$ mktouch() {mkdir -p $(dirname $1) && touch $1;}
```