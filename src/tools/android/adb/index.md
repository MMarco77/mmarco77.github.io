# Android Debug Bridge (`adb`)

- [Android ADB Cheat Sheet¶][1]
- [adb (Android Debug Bridge) cheatsheet][2]


# Basic Command

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb devices</td>
            <td>Lists connected devices</td>
        </tr>
        <tr>
            <td>adb connect 192.168.2.1</td>
            <td>Connects to adb device over network</td>
        </tr>
        <tr>
            <td>adb root</td>
            <td>Restarts adbd with root permissions</td>
        </tr>
        <tr>
            <td>adb start-server</td>
            <td>Starts the adb server</td>
        </tr>
        <tr>
            <td>adb kill-server</td>
            <td>Kills the adb server</td>
        </tr>
        <tr>
            <td>adb reboot</td>
            <td>Reboots the device</td>
        </tr>
        <tr>
            <td>adb shell</td>
            <td>Starts the backround terminal</td>
        </tr>
        <tr>
            <td>adb devices -l</td>
            <td>List of devices by product/model</td>
        </tr>
        <tr>
            <td>adb -s <code>&lt;deviceName&gt; &lt;command&gt;</code></td>
            <td>Redirect command to specific device</td>
        </tr>
        <tr>
            <td>adb –d <code>&lt;command&gt;</code></td>
            <td>Directs command to only attached USB device</td>
        </tr>
        <tr>
            <td>adb –e <code>&lt;command&gt;</code></td>
            <td>Directs command to only attached emulator</td>
        </tr>
    </tbody>
</table>

# Package Installation

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>adb shell install</strong> <code>&lt;apk&gt;</code></td>
            <td><strong>Install app</strong></td>
        </tr>
        <tr>
            <td><strong>adb shell install <code>&lt;path&gt;</code></strong></td>
            <td><strong>Install app from phone path</strong></td>
        </tr>
        <tr>
            <td>adb shell install -r <code>&lt;path&gt;</code></td>
            <td>Install app from phone path</td>
        </tr>
        <tr>
            <td>adb shell uninstall <code>&lt;name&gt;</code></td>
            <td>Remove the app</td>
        </tr>
    </tbody>
</table>

# Path

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb shell ls</td>
            <td>List directory contents</td>
        </tr>
        <tr>
            <td>adb shell ls -s</td>
            <td>Print size of each file</td>
        </tr>
        <tr>
            <td>adb shell ls -R</td>
            <td>List subdirectories recursively</td>
        </tr>
        <tr>
            <td><strong>adb shell pm path <code>&lt;package name&gt;</code></strong></td>
            <td><strong>Get full path of a package</strong></td>
        </tr>
        <tr>
            <td><strong>adb shell pm list packages -f</strong></td>
            <td><strong>Lists all the packages and full paths</strong></td>
        </tr>
    </tbody>
</table>

# File Operations

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>adb push <code>&lt;local&gt; &lt;remote&gt;</code></strong></td>
            <td><strong>Copy file/dir to device</strong></td>
        </tr>
        <tr>
            <td><strong>adb pull <code>&lt;remote&gt; &lt;local&gt;</code></strong></td>
            <td><strong>Copy file/dir from device</strong></td>
        </tr>
        <tr>
            <td>run-as <code>&lt;package&gt;</code> cat <code>&lt;file&gt;</code></td>
            <td>Access the private package files</td>
        </tr>
    </tbody>
</table>

# Phone Info

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb get-statе</td>
            <td>Print device state</td>
        </tr>
        <tr>
            <td>adb get-serialno</td>
            <td>Get the serial number</td>
        </tr>
        <tr>
            <td>adb shell dumpsys iphonesybinfo</td>
            <td>Get the IMEI</td>
        </tr>
        <tr>
            <td>adb shell netstat</td>
            <td>List TCP connectivity</td>
        </tr>
        <tr>
            <td>adb shell pwd</td>
            <td>Print current working directory</td>
        </tr>
        <tr>
            <td>adb shell dumpsys battery</td>
            <td>Battery status</td>
        </tr>
        <tr>
            <td>adb shell pm list features</td>
            <td>List phone features</td>
        </tr>
        <tr>
            <td>adb shell service list</td>
            <td>List all services</td>
        </tr>
        <tr>
            <td>adb shell dumpsys activity <code>&lt;package&gt;/&lt;activity&gt;</code></td>
            <td>Activity info</td>
        </tr>
        <tr>
            <td>adb shell ps</td>
            <td>Print process status</td>
        </tr>
        <tr>
            <td>adb shell wm size</td>
            <td>Displays the current screen resolution</td>
        </tr>
    </tbody>
</table>

# Package Info

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb shell list packages</td>
            <td>Lists package names</td>
        </tr>
        <tr>
            <td>adb shell list packages -r</td>
            <td>Lists package name + path to apks</td>
        </tr>
        <tr>
            <td>adb shell list packages -3</td>
            <td>Lists third party package names</td>
        </tr>
        <tr>
            <td>adb shell list packages -s</td>
            <td>Lists only system packages</td>
        </tr>
        <tr>
            <td>adb shell list packages -u</td>
            <td>Lists package names + uninstalled</td>
        </tr>
        <tr>
            <td>adb shell dumpsys package packages</td>
            <td>Lists info on all apps</td>
        </tr>
        <tr>
            <td>adb shell dump <code>&lt;name&gt;</code></td>
            <td>Lists info on one package</td>
        </tr>
        <tr>
            <td>adb shell path <code>&lt;package&gt;</code></td>
            <td>Path to the apk file</td>
        </tr>
    </tbody>
</table>

# Device Related Commands

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb reboot-recovery</td>
            <td>Reboot device into recovery mode</td>
        </tr>
        <tr>
            <td>adb reboot fastboot</td>
            <td>Reboot device into recovery mode</td>
        </tr>
        <tr>
            <td>adb shell screencap -p "/path/to/screenshot.png"</td>
            <td>Capture screenshot</td>
        </tr>
        <tr>
            <td>adb shell screenrecord "/path/to/record.mp4"</td>
            <td>Record device screen</td>
        </tr>
        <tr>
            <td>adb backup -apk -all -f backup.ab</td>
            <td>Backup settings and apps</td>
        </tr>
        <tr>
            <td>adb backup -apk -shared -all -f backup.ab</td>
            <td>Backup settings, apps and shared storage</td>
        </tr>
        <tr>
            <td>adb backup -apk -nosystem -all -f backup.ab</td>
            <td>Backup only non-system apps</td>
        </tr>
        <tr>
            <td>adb restore backup.ab</td>
            <td>Restore a previous backup</td>
        </tr>
        <tr>
            <td>-------</td>
            <td>-----------</td>
        </tr>
        <tr>
            <td>adb shell am start -a android.intent.action.VIEW -d URL</td>
            <td>Opens URL</td>
        </tr>
        <tr>
            <td>adb shell am start -t image/* -a android.intent.action.VIEW</td>
            <td>Opens gallery</td>
        </tr>
        <tr>
            <td>adb shell monkey -p app.package.name</td>
            <td>Starts the specified package</td>
        </tr>
    </tbody>
</table>

# INTENTS

[am Command](https://developer.android.com/studio/command-line/shell.html#am)

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>am start -a android.intent.action.VIEW -d https://github.com</td>
            <td>URI</td>
        </tr>
        <tr>
            <td>am start -a "android.intent.action.SEND" --es "android.intent.extra.TEXT" "Hello World" -t "text/plain"</td>
            <td>Mime Type and Extra string</td>
        </tr>
        <tr>
            <td>am start -n "your.application.packagename/path.to.the.Activity"</td>
            <td>Activity</td>
        </tr>
        <tr>
            <td>am start -n "your.application.packagename/path.to.the.Activity" - e "key" "data"</td>
            <td>Activity with extras</td>
        </tr>
        <tr>
            <td>am startservice -n "com.example.application/.BackgroundService"</td>
            <td>Service</td>
        </tr>
        <tr>
            <td>am broadcast -a "android.intent.action.PACKAGE_FIRST_LAUNCH" -d "com.example.application"</td>
            <td>Broadcast with Action</td>
        </tr>
        <tr>
            <td>
            am broadcast -a com.google.android.c2dm.intent.RECEIVE -n <code>&lt;YOUR_PACKAGE_NAME&gt;/&lt;YOUR_RECEIVER_NAME&gt;</code> -e "<code>&lt;EXTRA_KEY_1&gt;</code>"
            </td>
            <td>Notification (1)</td>
"<EXTRA_VALUE_1>" -e "<EXTRA_KEY_2>" "<EXTRA_VALUE_2>"</td>
        </tr>
    </tbody>
</table>

(1) [stackoverflow](http://stackoverflow.com/questions/27800369/simulating-android-gcm)

# Log

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>adb logcat <code>[options] [filter] [filter]</code></strong></td>
            <td><strong>View device log</strong></td>
        </tr>
        <tr>
            <td>adb bugreport</td>
            <td>Print bug reports</td>
        </tr>
        <tr>
            <td>adb logcat</td>
            <td>Starts printing log messages to stdout</td>
        </tr>
        <tr>
            <td>adb logcat -g</td>
            <td>Displays current log buffer sizes</td>
        </tr>
        <tr>
            <td>adb logcat -G <code>&lt;size&gt;</code></td>
            <td>Sets the buffer size (K or M)</td>
        </tr>
        <tr>
            <td>adb logcat -c</td>
            <td>Clears the log buffers</td>
        </tr>
        <tr>
            <td>adb logcat *:V</td>
            <td>Enables ALL log messages (verbose)</td>
        </tr>
        <tr>
            <td>adb logcat -f <code>&lt;filename&gt;</code></td>
            <td>Dumps to specified file</td>
        </tr>
    </tbody>
</table>

- Example

```shell
$ adb logcat -G 16M
$ adb logcat *:V > output.log
```

# Permissions

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb shell permissions groups</td>
            <td>List permission groups definitions</td>
        </tr>
        <tr>
            <td>adb shell list permissions -g -r</td>
            <td>List permissions details</td>
        </tr>
    </tbody>
</table>

# Android Version

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb shell getprop ro.build.version.release</td>
            <td>Get Build Version of Release Mode</td>
        </tr>
    </tbody>
</table>

# Proxy

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>adb shell settings put global http_proxy <code>&lt;address&gt;:&lt;port&gt;</code></td>
            <td>Set network proxy</td>
        </tr>
        <tr>
            <td>adb shell settings put global http_proxy :0
</td>
            <td>Disable network proxy</td>
        </tr>
    </tbody>
</table>


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

[1]: https://www.automatetheplanet.com/adb-cheat-sheet/
[2]: https://3os.org/android/adb-cheat-sheet/#push-a-file-to-download-folder-of-the-android-device