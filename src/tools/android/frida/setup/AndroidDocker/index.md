# Android Emulator/Frida 

Inspired from [Lairue Wired](https://www.youtube.com/@lauriewired)

## Docker

- Docker pul

> docker pull budtmo/docker-android:latest

[docker-android](https://github.com/budtmo/docker-android)

- Docker run

```shell
# docker run --privileged -d -p 6080:6080 -p 5554:5554 -p 5555:5555 -e DEVICE="Samsung Galaxy S6" --name android-container budtmo/docker-android-x86-8.1
$ docker run -d -p 6080:6080 -p 5554:5554 -p 5555:5555 -e EMULATOR_DEVICE="Samsung Galaxy S10" -e WEB_VNC=true --device /dev/kvm --name android-container budtmo/docker-android:latest
```

- Connect to shell

```shell
➜ adb connect 127.0.0.1:5555
* daemon not running; starting now at tcp:5037
* daemon started successfully
connected to 127.0.0.1:5555
➜ adb -s 127.0.0.1:5555 shell       
emu64xa:/ $
➜ adb -s 127.0.0.1:5555 root
restarting adbd as root
➜ adb -s 127.0.0.1:5555 shell 
emu64xa:/ # 
```

## Setup Frida

```shell
➜ pip show frida
Name: frida
Version: 16.1.4
Summary: Dynamic instrumentation toolkit for developers, reverse-engineers, and security researchers
Home-page: https://frida.re
Author: Frida Developers
Author-email: oleavr@frida.re
License: wxWindows Library Licence, Version 3.1
Location: /home/user/.pyenv/versions/3.10.10/envs/frida_env/lib/python3.10/site-packages
Requires: typing-extensions
Required-by: frida-tools
```

- For the emulator => Frida server `x86`

```shell
➜ unxz frida-server-16.1.4-android-x86_64.xz
➜ adb -s 127.0.0.1:5555 push frida-server-16.1.4-android-x86_64 /data/local/tmp
frida-server-16.1.4-android-x86_64: 1 file pushed, 0 skipped. 46.9 MB/s (51624572 bytes in 1.050s)
➜ adb -s 127.0.0.1:5555 shell 
emu64xa:/ # cd /data/local/tmp
127|emu64xa:/data/local/tmp # ls
frida-server-16.1.4-android-x86_64
128|emu64xa:/data/local/tmp # chmod +x ./frida-server-16.1.4-android-x86_64
➜ adb -s 127.0.0.1:5555 shell /data/local/tmp/frida-server-16.1.4-android-x86_64 -D &
[1] 690322
➜ adb -s 127.0.0.1:5555 shell                                                        
1|emu64xa:/ # netstat -tlp                                                                                            
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program Name
tcp        0      0 localhost:27042         0.0.0.0:*               LISTEN      6435/frida-server-16.1.4-android-x86_64
tcp        0      0 10.0.2.16:43826         par10s41-in-f4.1e:https CLOSE_WAIT  2550/com.google.android.googlequicksearchbox:search
tcp        0      0 10.0.2.16:41458         par10s41-in-f4.1e:https CLOSE_WAIT  2550/com.google.android.googlequicksearchbox:search
tcp6       0      0 [::]:5555               [::]:*                  LISTEN      6152/adbd
tcp6       0      0 ::ffff:10.0.2.16:46468  wl-in-f188.1e100.n:5228 ESTABLISHED 5168/com.google.android.gms.persistent
```

## Frida quick start

- Running process list 

> frida-ps -U

- Running application

> frida-ps -Ua

- Attach to a process

```shell
➜ frida-ps -Ua   
 PID  Name         Identifier                             
----  -----------  ---------------------------------------
2550  Google       com.google.android.googlequicksearchbox
2550  Google       com.google.android.googlequicksearchbox
5482  Messages     com.google.android.apps.messaging      
1581  Phone        com.google.android.dialer              
6074  Photos       com.google.android.apps.photos         
1085  SIM Toolkit  com.android.stk                        
6041  Settings     com.android.settings
6499  YouTube      com.google.android.youtube             
➜ frida -U Youtube
     ____
    / _  |   Frida 16.1.4 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at https://frida.re/docs/home/
   . . . .
   . . . .   Connected to sdk gphone x86 64 (id=127.0.0.1:5555)
                                                                                
[sdk gphone x86 64::Youtube ]-> Exit
```

## Sample

- Name: [Android](https://bazaar.abuse.ch/sample/c81234b6ceb3572c6d862a9313e019b98efd83165d8c085bd3e74971c66763bb/)
- SHA256: c81234b6ceb3572c6d862a9313e019b98efd83165d8c085bd3e74971c66763bb
- ZIP password: infected

### Script to hook

```js
Java.perform(() => {
    const wwuClass = Java.use('com.first.smoke.WWuToNtMwQcZlXnFwNx');

    wwuClass.arrivesample.implementation = function () {
        send('arrivesample() got called! Let\'s call the original implementation');
        var retValue = this.arrivesample();
        send('arrivesample() got called! Let\'s call the original implementation');
        return retValue;
    };
});
```

