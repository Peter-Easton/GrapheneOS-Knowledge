# Testing the Android Compatibility Test Suite

## Initial preparation

You're going to need a few things to test GrapheneOS.

### Hardware
* An x86 computer running your flavour of Linux. It doesn't have to be powerful, as the phone will do most of the work.
* A good-quality USB-A to USB-C cable. **Take Note:** Android takes full advantage of all the features offered by the USB protocol. Many laptops that have USB-C ports, or computer towers that have front ports often do not implement all the features, and Linux kernels that predate 5 may have broken support for USB 3 and 3.1. Many errors are caused by improperly traced or shoddy cables. For best results, always use a high-quality USB-A to C cable plugged directly into the motherboard of your computer.
* The device you want to test
* A well-lit area with a way to prop the phone up at least an arm's length away from something it can focus on. A great way to do this is simply using tape to secure the phone to something heavy, such as a wooden block, a small weight, or even a mitten drying rack.


### Software

On the software side of things, you're going to need

* The compatibility test suite software, which is located at: `https://dl.google.com/dl/android/cts/android-cts-10_r3-linux_x86-arm.zip`
* The compatibility test media, which you can also download at: `https://dl.google.com/dl/android/cts/android-cts-media-1.4.zip`
* The Android Studio, which is available for download at: `https://developer.android.com/studio/` If you're feeling brave, you can simply download it directly from the mirror at `https://redirector.gvt1.com/edgedl/android/studio/ide-zips/3.6.3.0/android-studio-ide-192.6392135-linux.tar.gz`

Or you can obtain it all at once by running:

```
user@computer: ~ $ wget https://dl.google.com/dl/android/cts/android-cts-10_r3-linux_x86-arm.zip https://dl.google.com/dl/android/cts/android-cts-media-1.4.zip https://redirector.gvt1.com/edgedl/android/studio/ide-zips/3.6.3.0/android-studio-ide-192.6392135-linux.tar.gz
```

### Setting up
Android Studio will need to be set up, as the Compatibility Test Suite depends on some software packages included in Android Studio. Simply download the Android Studio and run `studio.sh` in the `bin` directory to get it set up for your system. By default, it will save itself in `$HOME/Android`. You'll want to add this to your command path by running:

```
user@computer: ~ $ tar -xf android-studio-ide-192.6392135-linux.tar.gz
user@computer: ~ $ ./android-studio/bin/studio.sh
```
And following the instructions. By default, it will install to a directory called `Android` in your home directory. Unless you change the defaults, you should be able to add the tools to your command path using the command:
```
user@computer: ~ $ export PATH=$PATH:$HOME/Android/Sdk/platform-tools/:$HOME/Android/Sdk/tools:$HOME/Android/Sdk/tools/bin:$HOME/Android/Sdk/build-tools/29.0.2
```
If you would like this to be done automatically for you after closing the window, run the following:
```
user@computer: ~ $ echo "export PATH=$PATH:$HOME/Android/Sdk/platform-tools/:$HOME/Android/Sdk/tools:$HOME/Android/Sdk/tools/bin:$HOME/Android/Sdk/build-tools/29.0.2" >> ~/.bashrc
```
You can then unpack the compatibility test suite to your computer and appending it to your path by running the following:
```
user@computer: ~ $ unzip android-cts-10_r3-linux_x86-arm.zip
user@computer: ~ $ export PATH=$PATH:$(pwd)/android-cts/tools/:
```
If you'd like to make it persistent, you can run the following command:
```
user@computer: ~ $ echo "export PATH=$PATH:$(pwd)/android-cts/tools/:" >> ~/.bashrc
```
Now, you're just about all ready to run the CTS!

### Preparing your phone
Preparing the phone is relatively simple now that you've got the computer set up.

#### Turning on USB Debugging
In your phone, open up **Settings ➔ About Phone** and scroll all the way to **Build Number**. Tap on **Build Number** approximately ten times in rapid succession to unlock developer mode.

Return to **Settings** and go from **Settings ➔ System ➔ Advanced ➔ Developer Options** and turn on **USB Debugging.**

**Attention:** *Turning on USB Debugging opens up your phone to a significant amount of attack surface from the significantly less-secure computer being trusted to debug it, which can, once given permission to debug the phone, may execute a Unix-like shell on the device at will as well as both write, read, and erase files on the phone's shared storage. USB Debugging is only intended as a feature for debugging and testing only by developers. It is not intended to be used by "advanced users" to unlock additional functionality. Using USB Debugging on a device you intend to use daily is not recommended.

Connect the phone to your computer by the USB slot and drag down the top notification bar. You may see something that looks like "Charging this device via USB." Tap the bar to bring up the USB Preferences menu, which should have a multiple choice selection from "No data transfer." Set it to "File Transfer." Once you're done, type in:

```
user@computer: ~ $ adb devices
```

A box may pop up to authorize the identity of the computer plugged into the phone. You will need to approve it before any work can begin. You may see something like:
```
user@computer: ~ $ adb devices
* daemon not running; starting now at tcp:5037
* daemon started successfully
List of devices attached
XXXXXXXXX       device
```
If you can see this, you're ready to begin.

#### Transferring the CTS Media.
The CTS Media is a collection of videos, pictures, and files that the phone will use to debug itself for the CTS testing. If you've successfully set up USB debugging on your phone, you're now ready to begin transferring the CTS media to it, which uses a pair of scripts, and the software you've just provisioned and installed to send all the files to your phone in their proper place.

Simply unzip the CTS Media and run the scripts.
```
user@computer: ~ $ unzip android-cts-media-1.4.zip
user@computer: ~ $ ./android-cts-media-1.4/copy_images.sh && ./android-cts-media-1.4/copy_media.sh
```
Wait for them to complete. When they're ready, you're able to go ahead with testing.

#### Physically Setting Up Your Phone
Your phone should be in a well lit area, propped up on its side so both the front and rear cameras can focus on something. Ideally, each target should be brightly coloured, and at least an arm's length away from the phone. Taping the phone sideways to a brick, or placing it in a mitt drying rack is ideal. Make sure the phone is somewhere where it won't be disturbed.

**Attention:** *Your phone may at times produce loud squawking sounds or play loud sound effects, ringtones, or even movies. It may take pictures or even short video clips while it is being tested. This is normal and part of the Compatibility Test Suite. If you do not want it to play loud music, plug in a USB splitter and a set of USB headphones to prevent it from playing loud sounds.*

Prior to testing, you should ensure the phone has a few things set:
* It must be connected to a WiFi network
* You should have a working SIM card in it
* Disable SIM lock
* Enable Bluetooth
* Enable NFC
* Open Chromium and close it once
* You should have a good signal for GPS.
* You must turn off screen lock. A password or pincode will cause the CTS to fail.

### Running the CTS

Bring up the CTS with the command:

```
user@computer: ~ $ cts-tradefed
```
Which should bring you to the following screen:
```
==================
Notice:
We collect anonymous usage statistics in accordance with our Content Licenses (https://source.android.com/setup/start/licenses), Contributor License Agreement (https://opensource.google.com/docs/cla/), Privacy Policy (https://policies.google.com/privacy) and Terms of Service (https://policies.google.com/terms).
==================
Android Compatibility Test Suite 10_r3 (6221200)
Use "help" or "help all" to get more information on running commands.
cts-tf >
```
Welcome to the Android Compatibility Test Suite.

#### Running your first test

First, ensure your phone is propped up in a well-lit area.

There are hundreds of testing modules available, which can be run individually, or run as a set in a series of tests which are grouped together as plans. To list the modules in the CTS, at the prompt, type:
```
cts-tf > list modules
```
There are quite a large number of them! Don't be intimidated by the number of them, these are simply the individual test cases for each contingency the CTS has to test. To find a much more intuitive and manageable list of pre-bundled plans, simply type:
```
cts-tf > list plans
```
It's time to run your first test. At the prompt, type:
```
cts-tf > run cts
```
And let her rip. Note that you can expect that the testing can take several hours for the plan to run. At this time, the phone will be running tests, and even if it appears to be idle, should not be disturbed. Doing so may give false positives or false negatives on the test, and send all the time the phone has spent on the rack to waste. 

**Heads up!** *The CTS can be running for long periods of time and returns to the prompt while the job is running in the background. Long periods of time may go by when the CTS program will seem to be inactive. If you have any doubts as to what it's doing, find out its status by using the command:* `list invocations` *before attempting to cancel out of the CTS. As CTS batches can take long times to run, you may save yourself large amounts of time and frustration doing so and being patient, rather than simply assuming that the CTS has stopped or frozen, or isn't doing anything.*

#### After the test

After your test finishes, you'll be left with a directory listing your results in `android-cts/results` which are listed by date and time and are helpfully copied in zip files.

Open one of them, and you will see a file called `test_result.html` which can be opened in any web browser. This will tell you a summary of the results.

##### Things to keep note of:

You may want to run the test several times. The CTS is not exact and as it's a real world test of the phone's abilities, it may take running the same test plan up to five or more times to see what consistently passes and what consistently fail. Go through each test result report and be sure to take note of which tests consistently fail, the plans that were run, the time and builds that they were run on. This is important to the developers, and will help save them valuable time in analyzing each report themselves.

Ideally, the reports for the CTS should be compared between like devices: one running GrapheneOS, one running the Google Android, and one running standard AOSP.

More to come in the future.
