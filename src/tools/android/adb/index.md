# Android Debug Bridge (`adb`)

### Android Version

> adb shell getprop ro.build.version.release

### Package list

> adb shell 'pm list packages' | sed 's/.*://g'

### Get Screen state and locked state

> adb shell service call trust 7

### Input text

> adb shell input text S3cr37 && adb shell input keyevent 66

## Reboot device

> adb reboot

### Swipe Up

> adb shell input swipe 540 1600 540 100 150

### Start activities

- Throught `shell`

> adb shell 
> am start -n com.package.name/com.package.name.ActivityName

- Directly 

> adb shell am start -n com.package.name/com.package.name.ActivityName

- Intent-filter

> am start -a com.example.ACTION_NAME -n com.package.name/com.package.name.ActivityName 

- Androiderson tips

> adb shell monkey -p your.app.package.name 1

### Push file in temporary folder

> adb push \<FILE\> /data/local/tmp

### Install APK

> adb install \<APK-FILE\>
