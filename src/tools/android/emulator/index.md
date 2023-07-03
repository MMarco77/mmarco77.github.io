# Install and Create Emulators using AVDMANAGER and SDKMANAGER

## TL;DR

For an emulator that mimics a Pixel 5 Device with Google APIs and ARM/x86_64 architecture:

1. List All System Images Available for Download:

```shell
$ sdkmanager --list | grep system-images
```

2. Install platform tools, build tools, android platform:

```shell
$ echo "yes"  | sdkmanager "platform-tools" "build-tools;31.0.0" "platforms;android-31"
```

2. Install system images:

```shell
$ echo "yes" | sdkmanager "system-images;android-31;google_apis;x86_64"

```

3. Create Emulator:

```shell
echo "no" | avdmanager --verbose create avd \
                --force \
                --name "pixel_5_api30_google_atd_emulator" \
                --package "system-images;android-31;google_apis;x86_64" \
                --tag "google_atd" \
                --abi "arm64-v8a" \
                --device "pixel_5"
```

I recommend always using the new `google_atd` or `aosp_atd` images when possible. In my benchmarks, they are about 40% more efficient than the `google_apis` image.

**Note**: Available device (`--device`) list:

```shell
$ avdmanager list device
```

Passing in a device will make the Emulator settings (usually found in the `~/.android/avd/emulator.avd/config.ini` file) try to mimic that device. It is not actually the device. But, certain settings like pixel density, resolution, memory, partition size, etc will be changed. Generally the lower resolution devices will be less taxing on your CPU resources on CI, and are preferred especially without GPU/Hardware acceleration.

4. Run Emulator:

```shell
$ emulator @generic_api30_aosp_atd_emulator &
``` 
or 
```shell
emulator @pixel_5_google_atd_emulator &
```
5. Available adv

```shell
$ avdmanager list avd
```

### Aliases

Add aliases to your `~/.zshrc` or `~/.bashrc` to run the emulators with parameters more easily. 

```bash
alias pixel_5_wiped='emulator @pixel_5 -no-boot-anim -netdelay none -no-snapshot -wipe-data &'
```

### Starting multiple emulators

You can pass the `-read-only` parameter when starting up an emulator 

```shell
$ emulator @pixel_5 -read-only
```

to run multiple devices at the same time.

### Other

1. aosp_atd and google_atd system images are only available on x86 and ARM architecture at API level 30.

See also: https://android-developers.googleblog.com/2021/10/whats-new-in-scalable-automated-testing.html


```shell
$ sdkmanager --list | grep atd
  system-images;android-30;aosp_atd;arm64-v8a                                              | 1            | AOSP ATD ARM 64 v8a System Image
  system-images;android-30;aosp_atd;x86                                                    | 1            | AOSP ATD Intel x86 Atom System Image
  system-images;android-30;google_atd;arm64-v8a                                            | 1            | Google APIs ATD ARM 64 v8a System Image
  system-images;android-30;google_atd;x86                                                  | 1            | Google APIs ATD Intel x86 Atom System Image
```

## ensure PATH is correct

(if studio is in /opt/android-studio, and SDKs etc under ~/Android/Sdk):

```
ANDROID_STUDIO_LOC=/opt/android-studio
ANDROID_SDK_ROOT=~/Android/Sdk
PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_STUDIO_LOC/bin:$ANDROID_SDK_ROOT/emulator:$ANDROID_SDK_ROOT/tools:$ANDROID_SDK_ROOT/platform-tools:$PATH
```

## Docs

See: [Google's Emulator CLI Documentation](https://developer.android.com/studio/run/emulator-commandline) for more info.

See: [Google's AVDManager Documentation](https://developer.android.com/studio/command-line/avdmanager) for more info.

See: [Google's SDKManager Documentation](https://developer.android.com/studio/command-line/sdkmanager) for more info.