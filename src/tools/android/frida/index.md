# Frida

## Cheat sheet

### Installation

Install frida tools:

```shell
# Activate pyenv or venv
$ pip install frida-tools
$ pip install frida
```

Get last `frida server`

```shell
$ wget https://github.com/frida/frida/releases/download/VERSION/frida-server-VERSION-PLATFORM-ARCHI.xz 
```

Où : 

- `VERSION` is the last version (16.0.19 at Apr 27 2023)
- `PLATFORM` is one of [`android`|`windows`|`linux`]. This is the device target.
- `ARCHI` is [`arm`|`arm64`|`x86`|`x86_64`|`armeabi`]

Example:

For `Pixel 5, Android 12,  API 31 emulator` (from Android studio).

```shell
$ wget https://github.com/frida/frida/releases/download/16.0.19/frida-server-16.0.19-android-x86_64.xz
$ xz --decompress frida-server-16.0.19-android-x86_64.xz
$ mv frida-server-* frida-server
# Restart root
$ adb root
adbd is running as root
$ adb push frida-server /data/local/tmp/
frida-server: 1 file pushed, 0 skipped. 318.4 MB/s (112503224 bytes in 0.337s)
$ adb shell "chmod 755 /data/local/tmp/frida-server"
$ adb shell "/data/local/tmp/frida-server &" 
```

Check if it is working:

```shell
# List packages and processes
$ frida-ps -U 
# Get all the package name
$ frida-ps -U | grep -i <part_of_the_package_name>
```

## Reference

- Cheat Sheet
    - [Frida Ahndbook](https://learnfrida.info/)
    - Intigriti
        - [Github](https://github.com/carlospolop/hacktricks/blob/master/mobile-pentesting/android-app-pentesting/frida-tutorial/README.md)
        - [Blog](https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/frida-tutorial)
    - [Android Pentesting Using Frida](https://www.varutra.com/android-pentesting-using-frida/)
    - [Android reverse engineering for beginners - Frida](https://braincoke.fr/blog/2021/03/android-reverse-engineering-for-beginners-frida/#about-frida)
    - [Learning Frida](https://nibarius.github.io/learning-frida/)
    - [Frida Home](https://frida.re/docs/examples/android/)
- Tuto
    - hacktricks
        - [tuto1](https://medium.com/infosec-adventures/introduction-to-frida-5a3f51595ca1)
            - [From](https://medium.com/infosec-adventures/introduction-to-frida-5a3f51595ca1)
            - [APK](https://github.com/t0thkr1s/frida-demo/releases)
            - [Source Code](https://github.com/t0thkr1s/frida-demo)

        - [tuto2](https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/frida-tutorial/frida-tutorial-2)
            - [From](https://11x256.github.io/Frida-hooking-android-part-2/)
            - [APK and source](https://github.com/11x256/frida-android-examples)
        
        - [tuto3](https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/frida-tutorial/owaspuncrackable-1)
            - [From](https://joshspicer.com/android-frida-1)
            - [APK](https://github.com/OWASP/owasp-mstg/blob/master/Crackmes/Android/Level_01/UnCrackable-Level1.apk)
- [Awesome Frida scripts](https://codeshare.frida.re/)
