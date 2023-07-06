# Frida

## Frida Setup

    Sous python 3.9 et plus

### Setup python modules

```shell
# Pyenv or venv
$ pip install frida-tools
$ pip install frida
```

### Get Frida server

```shell
$ wget https://github.com/frida/frida/releases/download/VERSION/frida-server-VERSION-PLATFORM-ARCHI.xz 
```

Where : 

- `VERSION` is the last version (16.0.19 at Apr 27 2023)
- `PLATFORM` is one of [`android`|`windows`|`linux`]. This is the device target.
- `ARCHI` is [`arm`|`arm64`|`x86`|`x86_64`|`armeabi`]

Example:

For `Pixel 5, Android 12,  API 31 emulator` (from Android studio).

```shell
$ wget https://github.com/frida/frida/releases/download/16.0.19/frida-server-16.0.19-android-x86_64.xz
$ xz --decompress frida-server-16.0.19-android-x86_64.xz
$ mv frida-server-* frida-server
```

### Run frida server

```shell
# Restart root
$ adb root
adbd is running as root
$ adb push frida-server /data/local/tmp/
frida-server: 1 file pushed, 0 skipped. 318.4 MB/s (112503224 bytes in 0.337s)
$ adb shell "chmod 755 /data/local/tmp/frida-server"
$ adb shell "/data/local/tmp/frida-server -D &" 
```

### Get running process list

```shell
# List packages and processes
$ frida-ps -U 
# Get all the package name
$ frida-ps -U | grep -i <part_of_the_package_name>
```

## Reference

- Cheat Sheet
    - [Frida Handbook](https://learnfrida.info/)
    - Intigriti
        - [Github](https://github.com/carlospolop/hacktricks/blob/master/mobile-pentesting/android-app-pentesting/frida-tutorial/README.md)
        - [Blog](https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/frida-tutorial)
    - [Android Pentesting Using Frida](https://www.varutra.com/android-pentesting-using-frida/)
    - [Android reverse engineering for beginners - Frida](https://braincoke.fr/blog/2021/03/android-reverse-engineering-for-beginners-frida/#about-frida)
    - [Learning Frida](https://nibarius.github.io/learning-frida/)
    - [Frida Home](https://frida.re/docs/examples/android/)
- [Awesome Frida scripts](https://codeshare.frida.re/)
