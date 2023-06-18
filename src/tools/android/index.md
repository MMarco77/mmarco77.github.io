# Android

## Path (USer land)

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>/data/data/<code>&lt;package name&gt;</code>/databases</td>
            <td>App databases</td>
        </tr>
        <tr>
            <td>/data/data/<code>&lt;package name&gt;</code>/shared_prefs/</td>
            <td>Shared preferences</td>
        </tr>
        <tr>
            <td>/mnt/sdcard/Download/</td>
            <td>Download folder</td>
        </tr>
        <tr>
            <td>/data/app</td>
            <td>Apk installed by user</td>
        </tr>
        <tr>
            <td>/system/app</td>
            <td>Pre-installed APK files</td>
        </tr>
        <tr>
            <td>/mmt/asec</td>
            <td>Encrypted apps (App2SD)</td>
        </tr>
        <tr>
            <td>/mmt/emmc</td>
            <td>Internal SD Card</td>
        </tr>
        <tr>
            <td>/mmt/adcard</td>
            <td>External/Internal SD Card</td>
        </tr>
        <tr>
            <td>/mmt/adcard/external_sd</td>
            <td>External SD Card</td>
        </tr>
    </tbody>
</table>

## Usefull docker

[mingc/android-build-box](https://hub.docker.com/r/mingc/android-build-box) An optimized docker image includes Android, Kotlin, Flutter sdk. 

```shell
$ docker pull mingc/android-build-box
```


[Work in progress](https://hub.docker.com/r/mingc/android-build-box)