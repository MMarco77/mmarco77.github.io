# Frida setup

Frida can installed : 
- Rooted device(s)
- [Not rooted device(s)](./NotRooted/index.md)
- [Emulator(s)](./emulator/index.md)


### Setup python modules

Sous python 3.9 et plus

```shell
# Pyenv or venv
$ pip install frida-tools
$ pip install frida
```
### Get Frida server

```bash
VER=`frida --version`
ABI=`adb shell getprop ro.product.cpu.abi`
wget https://github.com/frida/frida/releases/download/$VER/frida-server-$VER-android-$ABI.xz
xz -d frida-server-$VER-android-$ABI.xz
mv frida-server-* frida-server
```

Where : 

- `VERSION` is the last version (16.0.19 at Apr 27 2023)
- `PLATFORM` is one of [`android`|`windows`|`linux`]. This is the device target.
- `ARCHI` is [`arm`|`arm64`|`x86`|`x86_64`|`armeabi`]

### Get Frida gadget

```bash
VER=`frida --version`
ABI=`adb shell getprop ro.product.cpu.abi`
wget https://github.com/frida/frida/releases/download/$VER/frida-gadget-$VER-android-$ABI.so.xz
xz -d frida-server-$VER-android-$ABI.xz
mv frida-server-* frida-server
```

Where : 

- `VERSION` is the last version (16.0.19 at Apr 27 2023)
- `PLATFORM` is one of [`android`|`windows`|`linux`]. This is the device target.
- `ARCHI` is [`arm`|`arm64`|`x86`|`x86_64`|`armeabi`]

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

### Run client script

```shell
frida -U -l <script>.js -f <package_name>
```
