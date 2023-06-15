# Android command

## `adb` cheat sheet

### Package list

> adb shell 'pm list packages' | sed 's/.*://g'

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