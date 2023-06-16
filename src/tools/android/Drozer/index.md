# Drozer

Drozer is a good tool for simulating a rogue application. F-Secure stopped its development, but it still can be used in your penetration test without any problems. From their GitHub page, there is an [APK][1] that should be installed on the device. The command is

```shell
$ adb install drozer-agent-2.3.4.apk
```

Drozer allows you to search for security vulnerabilities in apps and devices by assuming the role of an app and interacting with the Dalvik VM, other apps’ IPC endpoints, and the underlying OS. Even though it is decommissioned and out of development, I still use it as a go-to tool when it comes to mobile pentesting. More details can be found on their [GitHub][2].

```shell
$ wget https://github.com/WithSecureLabs/drozer/releases/download/2.4.4/drozer_2.4.4.deb
$ sudo dpkg -i drozer-2.4.4.deb
```

[1]: https://github.com/FSecureLABS/drozer/releases/download/2.3.4/drozer-agent-2.3.4.apk
[2]: https://github.com/WithSecureLabs/drozer