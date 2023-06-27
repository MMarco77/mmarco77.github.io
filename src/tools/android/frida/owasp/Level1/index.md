# Owasp Uncrackalbe level 1

## Reference

- [tuto1](https://medium.com/infosec-adventures/introduction-to-frida-5a3f51595ca1)
- [From](https://medium.com/infosec-adventures/introduction-to-frida-5a3f51595ca1)
- [APK](https://github.com/t0thkr1s/frida-demo/releases)
- [Source Code](https://github.com/t0thkr1s/frida-demo)

## Frida setup

Sous python 3.9 et plus

### Setup python modules

```shell
$ pip install frida-tools
$ pip install frida
```

### Get Frida server

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
$ adb shell "/data/local/tmp/frida-server &" 
```


```python
# Check if working
!frida-ps -U | grep "com.google"
```

    1270  com.google.android.apps.nexuslauncher                        
    4764  com.google.android.configupdater                             
     910  com.google.android.ext.services                              
    1045  com.google.android.gms.persistent                            
    3568  com.google.android.gms.unstable                              
    1558  com.google.android.googlequicksearchbox:interactor           
    1428  com.google.android.inputmethod.latin                         
    1328  com.google.android.permissioncontroller                      
    4949  com.google.android.projection.gearhead:shared                
    1527  com.google.android.providers.media.module                    
    3642  com.google.android.settings.intelligence                     
    3954  com.google.pixel.exo                                         
    1874  com.google.process.gapps                                     
    1512  com.google.process.gservices                                 


## Solve Unbreakable 1 (owasp challenge)

Throught `jadx-gui`, first step is `root detected`



```java
protected void onCreate(Bundle paramBundle)
{
if ((sg.vantagepoint.a.c.a()) || (sg.vantagepoint.a.c.b()) || (sg.vantagepoint.a.c.c())) {
a("Root detected!");
}

{.....truncated.....}
```

### Script frida to hook methods 'root detection'


```python
# %load ./disableRoot.js
Java.perform(function () {
    var theClass = Java.use("sg.vantagepoint.a.c");

    theClass.a.implementation = function(v) {
        console.log("In function A");
        return false
    }

    theClass.b.implementation = function(v) {
        console.log("In functions B");
        return false
    }

    theClass.c.implementation = function(v) {
        console.log("In function C");
        return false
    }
})
```

```shell
$ frida -U -l disableRoot.js -f owasp.mstg.uncrackable1
     ____
    / _  |   Frida 16.0.19 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at https://frida.re/docs/home/
   . . . .
   . . . .   Connected to Android Emulator 5554 (id=emulator-5554)
Spawned `owasp.mstg.uncrackable1`. Resuming main thread!                
[Android Emulator 5554::owasp.mstg.uncrackable1 ]-> In function A
In functions B
In function C
...
```

### Script frida for secret string

Code that check password:

```java
public static boolean a(String str) {
    byte[] bArr;
    byte[] bArr2 = new byte[0];
    try {
        bArr = sg.vantagepoint.a.a.a(b("8d127684cbc37c17616d806cf50473cc"), Base64.decode("5UJiFctbmgbDoLXmpL12mkno8HT4Lv8dlat8FxR2GOc=", 0));
    } catch (Exception e) {
        Log.d("CodeCheck", "AES error:" + e.getMessage());
        bArr = bArr2;
    }
    return str.equals(new String(bArr));
}
```

Called by:

```java
public static boolean a(String str) {
    byte[] bArr;
    byte[] bArr2 = new byte[0];
    try {
        bArr = sg.vantagepoint.a.a.a(b("8d127684cbc37c17616d806cf50473cc"), Base64.decode("5UJiFctbmgbDoLXmpL12mkno8HT4Lv8dlat8FxR2GOc=", 0));
    } catch (Exception e) {
        Log.d("CodeCheck", "AES error:" + e.getMessage());
        bArr = bArr2;
    }
    return str.equals(new String(bArr));
    }
```


```python
# %load ./crack.js
// Helper function to decode byte[] to String
function arrToStr(byteArr) {
    var tmp = "";
    for (var k = 0; k < byteArr.length; k++) {
    tmp += String.fromCharCode(byteArr[k]);
    }
    return tmp;
}

// Java.perform wraps all of our Frida code.
Java.perform(function() {
    // Detect root
    var classAC = Java.use("sg.vantagepoint.a.c");

    classAC.a.implementation = function(v) {
        console.log("In function A");
        return false
    };

    classAC.b.implementation = function(v) {
        console.log("In functions B");
        return false
    };

    classAC.c.implementation = function(v) {
        console.log("In function C");
        return false
    };

    console.log("Root Bypass Complete");

    var classAA = Java.use("sg.vantagepoint.a.a");
    // Execute a(byte[], byte[])
    classAA.a.implementation = function(x1, x2) {
        var rawFunctionCall = this.a(x1, x2);
        var password = arrToStr(rawFunctionCall);
        console.log("=> " + password);
        return password;
    };
});
```

Password:


    I want to believe
