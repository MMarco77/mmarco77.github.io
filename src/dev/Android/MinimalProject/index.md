# Minimal project in JAVA to create APK

Minimal requirement to create APK

Archive: [Project Template](./app.tar.gz)

## Project Structure

```
ProjectFolder
    ├── app
    │   └── src
    │       └── main
    │           ├── AndroidManifest.xml
    │           ├── java
    │           │   └── com
    │           │       └── example
    │           │           └── myapp
    │           │               ├── MainActivity.java
    │           │               └── MainApp.java
    │           └── res
    │               └── values
    │                   └── values.xml
    └── Makefile
```

## Files contains

- AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.example.myapp">
    <uses-sdk android:minSdkVersion="30" android:targetSdkVersion="34" />
    <application
        android:label="@string/myAppName">
        <activity
            android:name="com.example.myapp.MainActivity" 
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```
  
- MainActivity.java

```java
package com.example.myapp;

import android.os.Bundle;

import java.lang.reflect.Method;

import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.widget.TextView;

public class MainActivity extends Activity {
    public final static String TAG = "APP_AG";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        Log.i(TAG, "MainActivty called!");
        super.onCreate(savedInstanceState);
        TextView textView = new TextView(this);

        String text = getResources().getString(R.string.welcomeText);
        textView.setText(text);

        setContentView(textView);
    }
}
```

- MainApp.java

```java
package com.example.myapp;

import android.app.Application;
import android.util.Log;

public class MainApp extends Application  {
    @Override
    public void onCreate() {
        super.onCreate();

        Log.e("MyApp", "MyApp Application Start!\n");

    }

    @Override
    public void onTerminate() {
        super.onTerminate();
    }
}

```

- values.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="myAppName">MyApp example test</string>
    <string name="welcomeText">Welcome to my App 👋</string>
</resources>
```

- Makefile

```Makefile
PROJECT=MyPersonalProject

ANDROID=/home/user/Android
SDK=$(ANDROID)/Sdk
ANDROIDJAR=$(SDK)/platforms/android-34/android.jar
JAVA_FILES=app/src/main/java/com/example/myapp/*.java
RES_DIR=app/src/main/res
JAVA_SRC_DIR=app/src/main/java
MANIFEST=app/src/main/AndroidManifest.xml
BUILD_TOOLS_VERSION=33.0.2

KS_PASSWORD=myincredilbylongpasswordlol

# Tools
D8=$(SDK)/build-tools/$(BUILD_TOOLS_VERSION)/d8
AAPT=$(SDK)/build-tools/$(BUILD_TOOLS_VERSION)/aapt
ZIPALIGN=$(SDK)/build-tools/$(BUILD_TOOLS_VERSION)/zipalign
APKSIGNER=$(SDK)/build-tools/$(BUILD_TOOLS_VERSION)/apksigner

BUILD_DIR=build
KEYSTORE=$(BUILD_DIR)/tmp_key_store.ks
TARGET=$(PROJECT).apk

all: $(TARGET)

# Generate keystore to build the APK
$(KEYSTORE):
	keytool -genkey -v -keystore $@ -storepass $(KS_PASSWORD) -alias myapp_keystore -keyalg RSA -keysize 4096 -validity 10000

# Build res java files
res:
	$(AAPT) package 						\
		-f 									\
		-m 									\
		-M $(MANIFEST) 						\
		-S $(RES_DIR)		 				\
		-J $(JAVA_SRC_DIR) 					\
		-I $(ANDROIDJAR) 					\
		--generate-dependencies

# Build java classes
java: res
	mkdir -p $(BUILD_DIR)/obj
	javac --class-path $(ANDROIDJAR)  						\
		-source 1.8											\
		-target 1.8											\
		-d $(BUILD_DIR)/obj									\
		$(JAVA_SRC_DIR)/com/example/myapp/*.java


# Build classes.dex from java classes
$(BUILD_DIR)/bin/classes.dex: java
	mkdir -p $(BUILD_DIR)/bin
	$(D8) --classpath $(ANDROIDJAR)							\
		--output $(BUILD_DIR)/bin/							\
		./build/obj/com/example/myapp/*.class			

# Build APK with classes.dex and resources
$(BUILD_DIR)/$(TARGET).unaligned: $(BUILD_DIR)/bin/classes.dex $(MANIFEST)
	$(AAPT) package							\
		-f 									\
		-F $(BUILD_DIR)/$(TARGET).unaligned \
		-S $(RES_DIR)  						\
		-M $(MANIFEST) 						\
		-I $(ANDROIDJAR)					\
		$(BUILD_DIR)/bin

# Sign APK
$(TARGET): $(BUILD_DIR)/$(TARGET).unaligned $(KEYSTORE)
	$(ZIPALIGN) -f -p -z 4					\
		$(BUILD_DIR)/$(TARGET).unaligned	\
		$(BUILD_DIR)/$(TARGET).unsigned

	$(APKSIGNER) sign --ks $(KEYSTORE) --ks-pass pass:$(KS_PASSWORD) $(BUILD_DIR)/$(TARGET).unsigned
	mv $(BUILD_DIR)/$(TARGET).unsigned $(TARGET)

install: $(TARGET)
	adb install $(TARGET)

uninstall:
	adb uninstall com.example.myapp

clean:
	find app -name "R.java" -delete
	rm -rf $(BUILD_DIR)
	rm -rf $(TARGET)


```