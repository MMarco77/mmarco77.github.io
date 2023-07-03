# AVDManager (Android Virtual Device Manager)

## Android Emulator Crash

### `load libvulkan.so: failed`

- [Android Emulator does not start after Arctic Fox upgrade, libvulkan.so: failed](https://stackoverflow.com/questions/68741533/android-emulator-does-not-start-after-arctic-fox-upgrade-libvulkan-so-failed)

Go to emulator folder then change `gpu` mode:

```shell
$ cd $HOME/.android/avd/<EMULATOR-LABEL>.avd
$ sed -i 's/hw.gpu.mode = auto/hw.gpu.mode = guest/g' ./config.ini
```