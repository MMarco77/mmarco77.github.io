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
  ...
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

## Decode bytes to string

```javascript
// Helper function to decode byte[] to String
function arrToStr(byteArr) {
    var tmp = "";
    for (var k = 0; k < byteArr.length; k++) {
    tmp += String.fromCharCode(byteArr[k]);
    }
    return tmp;
}
```

## Exported, imported and symbols from module

- Lists all names (exports, imports and symbols) in the given module.
If a 'needle' is given this function returns the address of the name that matches the 'needle'

```javascript
function listNames(module, needle) {
    var address = undefined;
    Process.enumerateModules()
        .filter(function (m) { return m["path"].toLowerCase().indexOf(module) != -1; })
        .forEach(function (mod) {
            mod.enumerateExports().forEach(function (entry) {
                console.log("Export: " + entry.name);
                if (entry.name.indexOf(needle) != -1) address = entry.address;
            });
            mod.enumerateImports().forEach(function (entry) {
                console.log("Import: " + entry.name);
                if (entry.name.indexOf(needle) != -1) address = entry.address;
            });
            mod.enumerateSymbols().forEach(function (entry) {
                console.log("symbol name: " + entry.name);
                if (entry.name.indexOf(needle) != -1) address = entry.address;
            });
        });
    console.log("");
    return address;
}
```

## Hook function in library

- An `libfoo.so` use `strncmp` function.

```javascript
function listNames(module, needle) {
    var address = undefined;
    Process.enumerateModules()
        .filter(function (m) { return m["path"].toLowerCase().indexOf(module) != -1; })
        .forEach(function (mod) {
            mod.enumerateExports().forEach(function (entry) {
                if (entry.name.indexOf(needle) != -1) address = entry.address;
            });
            mod.enumerateImports().forEach(function (entry) {
                if (entry.name.indexOf(needle) != -1) address = entry.address;
            });
            mod.enumerateSymbols().forEach(function (entry) {
                if (entry.name.indexOf(needle) != -1) address = entry.address;
            });
        });
    return address;
}

function attachToStrncmp(strncmpAddress, compareTo) {
    Interceptor.attach(strncmpAddress, {
        onEnter: function (args) {
            var len = compareTo.length;
            if (args[2].toInt32() != len) {return;}
            var str1 = args[0].readUtf8String(len)
            var str2 = args[1].readUtf8String(len)
            if (str1 == compareTo || str2 == compareTo) {
                console.log("strncmp(" + str1 + ", " + str2 + ", " + len + ") called");
            }
        },
    });
}

Java.perform(function () {
    Java.use("<MODULE>.MainActivity").onResume.implementation = function () {
        this.onResume();

        // Hook to strncmp and look for any calls to it with the magic word as argument
        var watchDog = <STRING_UNIQUE_2_DISTNGUISHED_CALL>;
        var strncmp_addr = listNames('libfoo.so', 'strncmp')
        attachToStrncmp(strncmp_addr, theMagicWord);

        // Find the CodeCheck instance that was created by the app in onCreate()
        // Then call it with the magic word as argument.
        Java.choose("CLASS_MODULE", {
            onMatch: function (instance) {
                instance.<FCT_IN_CLASS>(
                    Java.use("java.lang.String").$new(watchDog)
                );
                return "stop";  // return 'stop'
            },
            onComplete: function () { }
        });

        // We've got what we wanted, detach again to prevent multiple attachments
        // to strncmp if onResume is called multiple times.
        Interceptor.detachAll();
    }
})
```

## Other Snippet

- [FridaLab – Writeup](https://www.shielder.com/it/blog/2019/02/fridalab-writeup/)

- [iddoeldor/frida-snippets [EXCELLENT]](https://github.com/iddoeldor/frida-snippets#class-description)

- [Solving OWASP MSTG UnCrackable App for Android Level 2](https://nibarius.github.io/learning-frida/2020/05/23/uncrackable2)

- [Frida Scripting Guide for Java](https://cmrodriguez.me/blog/frida-scripting-guide/)