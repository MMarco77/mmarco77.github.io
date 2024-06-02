# Frida

## Frida Tools

### frida-ps

- Get the list of running processes/applications

```shell
# -U option for USB devices or emulators
$ frida-ps -U
```

- Get the list of installed applications with -i option

```shell
$ frida-ps -U -i
```
## Frida CLI

- By default, Frida tries to attach to a running process

```shell
$ frida -U com.android.chrome
```

- To spawn an application, use the -f option as follow

```shell
$ frida -U -f com.android.chrome
```

- By default, the process is paused

```shell
$ frida -U -f com.android.chrome --no-pause
```

- Short usefull command

```shell
$ frida -U -l ./<script>.js -p `adb shell pidof -s <PACKAGE_NAME>`
```

## Frida Scripting

- Python bindings are provided with Frida
    - However, the hooks need to be written in JavaScript :(
- Here are the basics Frida functions
    - Java.perform(function() {//your code here});
        - Perform code instrumentation inside the Java code
    - Java.use(class_name)
        - Use a specific Java class
    - overload
        - Overload a specific method (functions with different prototypes)
    - implementation
        - Tamper the implementation of the selected method

- The most convenient way is to use scripts

```shell
$ frida -U -l myscript.js com.android.chrome
```

- Here is an example allowing to hook the onCreate function

```Javascript
Java.perform(function () {
    // Declare the Activity class as a variable
    var Activity = Java.use("android.app.Activity");
    
    // Hook the onCreate function
    Activity.onCreate.implementation = function () {
        console.log("[*] onCreate() got called!");
        this.onCreate();
    };
});
```

- When spawning an application, it is recommended to use Java.available as follow
    - Sometimes the JVM is not ready, and the script failed to execute

```Javascript
if(Java.available) {
    Java.perform(function () {
        // your code here
    };
}
```

- To print debug messages, you can use those functions 

```shell
$ send(message)
$ console.log(line) / console.warn(line) / console.error(line)
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
- [Learning Frida](https://nibarius.github.io/learning-frida/)