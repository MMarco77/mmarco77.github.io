# Frida Snippet Code

## Skeleton

```python
if (Java.available) {
  Java.perform(function() {
    # Code python
  }
}
```

## Methods call

- Static method

```javascript
var BasicTypes = Java.use("com.blog.testfrida.examples.BasicTypes"); 
BasicTypes.divideFloat(23,3);
```

- Non static method

```javascript
// Class reference
var Person = Java.use("com.blog.testfrida.complexobjects.Person");
// Create instance
var personInstance = Person.$new(); 
```

- Send string parameters to function

```javascript
var String = Java.use("java.lang.String"); 
personInstance.setName(String.$new("Peter Griffin"));
```

### Overridden methods

Here is overridden methods: 

```java
public static int multiply(int val1, int val2) {
    return val1 * val2;
}

public static byte multiply(byte val1, byte val2) {
    return (byte) (val1 * val2);
}
```

- Use in javascript will be:

```javascript
var BasicTypes = Java.use("com.blog.testfrida.examples.BasicTypes"); 
console.log(BasicTypes.multiply(3,5));
```

Frida solves the method by the argument types.

- Override methods

```javascript
var BasicTypes = Java.use("com.blog.testfrida.examples.BasicTypes"); 
console.log(BasicTypes.multiply(3,5));

// Failed to used this methods: which ones we are overridern
// BasicTypes.multiply.implementation = function (val1, val2) {
//    console.log("it works");
//    return val1 * val2;
//}
BasicTypes.multiply.overload('int','int').implementation = function (val1, val2) {
    return val1 * val2;
}
```

## Trace class

### List all overwritten methods and get count

```javascript
function traceClass(targetClass) {
    var hook;
    try {
        hook = Java.use(targetClass);
    } catch (e) {
        console.error("trace class failed", e);
        return;
    }

    var methods = hook.class.getDeclaredMethods();
    hook.$dispose();

    var parsedMethods = [];
    methods.forEach(function (method) {
        var methodStr = method.toString();
        var methodReplace = methodStr.replace(targetClass + ".", "TOKEN").match(/\sTOKEN(.*)\(/)[1];
         parsedMethods.push(methodReplace);
    });

    uniqBy(parsedMethods, JSON.stringify).forEach(function (targetMethod) {
        traceMethod(targetClass + '.' + targetMethod);
    });
}

function traceMethod(targetClassMethod) {
    var delim = targetClassMethod.lastIndexOf('.');
    if (delim === -1)
        return;

    var targetClass = targetClassMethod.slice(0, delim);
    var targetMethod = targetClassMethod.slice(delim + 1, targetClassMethod.length);

    var hook = Java.use(targetClass);
    var overloadCount = hook[targetMethod].overloads.length;

    console.log(targetClassMethod + " (overloadCount: " + overloadCount + ")");
}

// remove duplicates from array
function uniqBy(array, key) {
    var seen = {};
    return array.filter(function (item) {
        var k = key(item);
        return seen.hasOwnProperty(k) ? false : (seen[k] = true);
    });
}
```

- Usage

```javascript
traceClass("java.lang.String");
```

### List overwritten methods

```javascript
function listOverritenMethods(className, func) {
    var Class = Java.use(className);
    var overloadedMethods = Class[func].overloads;
    for (var i in overloadedMethods) {
        console.log(overloadedMethods[i]);
    }
}
```
- Usage 

```javascript
listOverritenMethods("java.lang.String", "getBytes");
```

## Function substitution

- Code that detect 'root'

```smali
protected void onCreate(Bundle paramBundle)
{
    if (sg.vantagepoint.a.c.c()) {
        a("Root detected!");
    }
```

- Frida Script

```javascript
Java.perform(function () {
    let theClass = Java.use("sg.vantagepoint.a.c");
    theClass.a.implementation = function(v) {
        console.log("In function A");
        return false
    }
})
```

or 

```javascript
Java.perform(function () {
    let MainActivity = Java.use("sg.vantagepoint.uncrackable.MainActivity");
    MainActivity.a.overload('java.lang.String').implementation = function (str) {
        console.log(`MainActivity.a is called: str=${str}`);
    };
})
```

**Rappel** Execute script

```shell
$ frida -U -l disableRoot.js -f owasp.mstg.uncrackable1
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


```javascript
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

## Other Snippet

- [FridaLab – Writeup](https://www.shielder.com/it/blog/2019/02/fridalab-writeup/)

- [iddoeldor/frida-snippets [EXCELLENT]](https://github.com/iddoeldor/frida-snippets#class-description)

- [Solving OWASP MSTG UnCrackable App for Android Level 2](https://nibarius.github.io/learning-frida/2020/05/23/uncrackable2)