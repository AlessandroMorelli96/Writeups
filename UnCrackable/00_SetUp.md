# Set Up

For this series of challenge we need this tools:

- Emulator
- adb
- jadx
- radare2

## Installation

```bash
brew install android-sdk android-platform-tools radare2 jadx
```

Then we need to install some tools, with the sdkmanager:

### Emulator

```bash
sdkmanager --install "platforms;android-29"
sdkmanager --install "system-images;android-29;default;x86_64"
sdkmanager --install "cmdline-tools;latest"
sdkmanager --install "emulator"
```

create a directory in the home folder named `.android` (`mkdir ~/.android`),
and set some variable in our zshrc/bashrc:

```text
export ANDROID_SDK_ROOT=/usr/local/share/android-sdk/
export ANDROID_AVD_HOME=~/.android/avd
```

then to create a new emulator, run:

```bash
avdmanager --verbose create avd --force --name "generic_10_64" --package "system-images;android-29;default;x86_64" --tag "default" --abi "x86_64"
```

Now to modify some settings we need to modify the file
`~/.android/avd/generic_10_64.avd/config.ini` and add:

```text
skin.name=1080x1920
hw.lcd.density=480
hw.keyboard=yes
```

now to run our emulator run:

```bash
/usr/local/share/android-sdk/emulator/emulator -no-boot-anim -netdelay none -accel on -no-snapshot -wipe-data -skin 1080x1920 -avd generic_10_64
```

### Frida

![Frida](https://frida.re/)

![Download](https://github.com/frida/frida/releases) the correct version for
your device in my case `frida-server...-android-x86_64`, and copy to the device,
than run it:

```bash
adb push frida-server /data/local/tmp/
adb shell
cd /data/local/tmp
chmod +x ./frida-server
su
./frida-server
```

On the host install frida-tools:

```bash
pip install frida-tools
```
