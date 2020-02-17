# Your GrapheneOS Shopping List
## Software
### Operating System
Building has been done successfully on the following operating systems:
* Debian Buster 10
* Ubuntu LTS 18.04
* Arch Linux
* Fedora 30
* Gentoo (Experimental only, requires modifications)

Building on Macintosh and Windows is not supported.

### Operating System Packages
You should consider installing these packages from your operating system's repositories. Install them first.
* `git`
* Python interpreters. At this moment, Google requires both python 3 and python 2.7, which is named differently on different distros. On Arch, a symlink must be made to link python to python2.7.
* `ninja-build`
* A Java development kit (on Debian, this is `default-jdk`)

Android uses its own in-tree toolchain that forbids clang, clang++, gcc, and g++. The details can be checked out here:
https://android.googlesource.com/platform/build/soong/+refs/tags/android-10.0.0_r11/ui/build/paths/config.go#76

#### For building Auditor
If you are building Auditor, you will need (in addition to the above)
* `gradle`
* `maven`
* `ant`

### Android Applications
#### Android Studio
You can obtain the Android Studio directly from Google, which will include the Platform Tools for Android Debug Bridge and Fastboot. Since this version is frequently updated it cannot be hotlinked here.
https://developer.android.com/studio/index.html#downloads

Once you download, unzip, and install it, ensure that it is properly added to your `$PATH` variable. This includes the following locations from where the Android SDK was installed to the following locations. These are the defaults:
* `$HOME/Android/Sdk/tools`
* `$HOME/Android/Sdk/tools/bin`
* `$HOME/Android/Sdk/build-tools/29.0.2`
* `$HOME/Android/Sdk/platform-tools/`

You can set the locations automatically on each new shell you open with:
```
echo "export PATH=$PATH:$HOME/Android/Sdk/tools:$HOME/Android/Sdk/tools/bin:$HOME/Android/Sdk/build-tools/29.0.2:$HOME/Android/Sdk/platform-tools/" >> .bashrc && export PATH=$PATH:$HOME/Android/Sdk/tools:$HOME/Android/Sdk/tools/bin:$HOME/Android/Sdk/build-tools/29.0.2:$HOME/Android/Sdk/platform-tools/
```

#### Android Platform Tools
**If you obtained Android Studio, you do not need Platform Tools, as it is already included.**
The Android Platform tools include Android Debug Bridge and Fastboot. These utilities are required for installing GrapheneOS, and they are also required for doing development work.

If you are running Windows or Macintosh, you can obtain the platform tools archive by visiting:
https://developer.android.com/studio/releases/platform-tools

**Linux users, beware!**

**Many Linux Distributions package their own versions of Android Debug Bridge and Fastboot. Unless stated specifically here, it is never safe to use them with modern devices running Android. They will often be out of date and will be versioned incorrectly, which may result in them failing to negotiate with the device silently, and *bricking* your device.**

Don't take the risk. On Linux, you can obtain the latest Platform Tools package, by running `wget https://dl.google.com/android/repository/platform-tools-latest-linux.zip && unzip platform-tools-latest-linux.zip`

**"I use Arch, btw"**

Arch uses up to date and correctly numbered versions of the Android Platform Tools and udev rules. If you use Arch, you can simply install the `android-tools` and `android-dev` AUR packages.

#### Android udev Rules
On Linux, you may find that Android Debug Bridge and Fastboot may be failing to detect your phone, even though it will show up in `lsusb` when plugged in. Some distributions do not adequately maintain their own udev rules for modern devices. You may obtain more up to date udev rules using the command sequence below:
```
$ wget https://raw.githubusercontent.com/M0Rf30/android-udev-rules/master/51-android.rules && sudo cp 51-android.rules /etc/udev/rules.d/51-android.rules && sudo udevadm control --reload-rules
```

You will need to add yourself to the `adbusers` usergroup, with `export MY_USERNAME=$(whoami) && sudo usermod -G adbusers $MY_USERNAME`

Log off and log back in to ensure the settings take effect.

#### Android Compatibility Test Suite
You should download the Compatibility Test Suite directly from Google and make sure it is the latest version. You can run the following commands to do it.

```
$ mkdir -p ~/GrapheneOS/CTS/ && cd ~/GrapheneOS/CTS
$ wget https://dl.google.com/dl/android/cts/android-cts-10_r1-linux_x86-arm.zip && unzip android-cts-10_r1-linux_x86-arm.zip
$ wget https://dl.google.com/dl/android/cts/android-cts-media-1.4.zip && unzip android-cts-media-1.4.zip
```

Scripts for copying the CTS test cases is included in the CTS media. Running these scripts requires that you have platform tools configured and working and the platform tools in your `$PATH`

#### Chromium Depot Tools
To configure and build GrapheneOS, you will need the Chromium Depot tools, and need to have the Chromium Depot tools in your `$PATH` variable in order to properly build. The easiest way to do this is to clone them from the Google.

```
$ mkdir -p ~/GrapheneOS/depot-tools/ && cd ~/GrapheneOS/depot-tools/
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
$ echo "export PATH=$PATH:$HOME/GrapheneOS/depot-tools" >> .bashrc && export PATH=$PATH:$HOME/GrapheneOS/depot-tools
```

## Hardware you will need
### Your workstation
When shopping around for a workstation, consider the following:
* You will need an Intel or AMD x86_64(amd64) processor. The more cores, and the faster it is, the better. Building the operating system (without building Vanadium, which is required to build GrapheneOS) can take approximately two and a half hours on an AMD Ryzen 2600 6-core system when memory and disk usage is not a bottleneck. Builds may need to be done and tested frequently so consider that time is money and potential savings in time will offset the cost of a more expensive processor.
* At least 16 GB of RAM. As with the CPU, bigger is better. 32 GB is recommended. 
* SSD with at least 300GB of free space. The more the better. Buying a 1TB SSD is recommended although it's possible to get away with using 500GB if you are careful, but this is inconvenient. Buy more than what you need. 
* Adequate cooling and ventilation, as your CPU will be running at 100% frequency for several hours during doing a full build.
Graphics cards are not required, as the build process does not involve any input from the Graphics card. This allows debugging and testing builds to be outsourced to a headless build server, or even a cloud service, but this is not something I will get into in this page.

### The device being developed for
Although some applications can be run on the emulator, the devices require maintainers and often require significant amounts of per-device work to be done to take advantage of the security features, exploit mitigations, and hardening specific to each device, so access to the device you intend to develop for is not optional, it is a necessity.

### CTS testing rig
When running the Compatibility Test Suite, you should prop the phone up between two objects both the front and rear cameras can focus on simultaneously, as both the front and rear cameras are tested during the camera test cases. A good method is to use a small piece of rubber tape to hold the phone on its side and tape it to something heavy like the bottom of a large saucer or a small pie dish turned upside-down to create a stand, or even making a stand out of something like Lego.

In order for the photo test cases to work, both the front and rear cameras must be able to focus on their targets. A brightly-coloured target with a high amount of contrast is recommended, and it is important to make sure the surfaces of the targets are not reflective or shiny.

## Accessories that may come in handy

### A phone
You will want to have a phone that is not your daily driver device for testing and debugging GrapheneOS. As this device may need to be unlocked, relocked, and reset often, it is strongly recommended not to use your daily driver for development.

### Second spare phone for running the factory OS
If you are testing builds using the Compatibility Test Suite, it may help to benchmark a device running GrapheneOS against an identical device running the factory operating system. Having an extra phone may also help if you depend on your device as a daily driver. Expect to have to unlock your bootloader often, which makes using the same phone as the one you are working on for daily use problematic.

For developers doing Auditor or application level development, having a spare phone (even if unsupported) that runs the factory operating system may help with debugging and testing of the app.

### Your connection
Expect to download several hundred gigabytes of source and history the first sync and large amounts of bandwidth may be used to sync the source repositories. Plan accordingly when it comes to your Internet use plan.

### A debugging cable
The Suzy-Qable that is sold as a "ChromeOS Debugging Cable" can obtain the citadel logs from the Pixel 2 and Pixel 3 series phones, but it does not have the chip which allow it to obtain the kernel uart debugging logs. Google does not sell these Kernel Debugging Cables to the public, leaving the only option to build one yourself out of a USB-C breakout board, a USB C cable, and a USB to Serial TTL Cable. A step by step tutorial including where to purchase the materials is available at https://github.com/Peter-Easton/android-debug-cable-howto I will improve this as time goes on.

### A cheap spare laptop
Since running the compatibility test suite can require moving the phone to a well-lit area where it can take photographs from both its front and rear camera and run undisturbed for several hours at a time, it may help with getting a cheap old laptop for running the compatibility test cases. Since the phone does all the hard work, even an old Dual-Core laptop running Linux will do fine as long as it won't break down through the tests. 

### Well lit area for running compatibility test suite.
Running the compatibility test suite will require a well-lit area with something that both the front and rear cameras can focus on simultaneously which can remain brightly lit for long periods of time. The phone's cameras will need something to focus on, and bright, sharply-contrasting colours are preferred, so flags or brightly-coloured clothes work great for this. Running the test suite can take several hours at a time for only a few tests, and once started should not be interrupted. 

The phone must be propped up (Simply taping your phone to a clipboard works well for this) inbetween the targets, as both the front and rear cameras need to focus on something at the same time.
