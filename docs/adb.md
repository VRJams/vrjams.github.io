---
layout: default
parent: Docs
title: ADB
nav_order: 4
---

# ADB
ADB or [Android Debug Bridge](https://developer.android.com/studio/command-line/adb) is one of the key utilities used by Android developers and power users to control Android to its core without rooting.
It's a key tool to use LOVR effectively on your Android HMD, making it much faster to debug, install, update and much more.

To use it, you need three steps first:
1. activate ADB on the HMD
2. install ADB on your PC
3. connect the devices 

## ADB on the HMD
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

ADB is __usually__ hidden in the Developer Settings of the device, but it's not a standard process to activate it.

### Oculus

This process is a bit involved, as Meta requires users to register as developers. It's not hard, just tedious.

1. Create an organization on [Meta's page](https://developer.oculus.com/manage/organizations/create/) with a valid Meta account, which you can create unlinked from any Facebook profiles
2. Install the Oculus App on your phone
3. Link the app to the HMD and your account, linked to the organization
4. Activate **developer Mode** setting in the **Headset Settings** in the app 
5. In your HMD, in **Settings**, find **Developer** and activate **USB Connection Dialogue**

Now your headset can use ADB!
This process can also be done without a phone using the [Oculus Developer Hub](https://developer.oculus.com/documentation/unity/ts-odh/) program 

For a more step-by-step guide, you can read [here](https://www.wikihow.com/Enable-Developer-Mode-Oculus-Quest-2)

### Pico 4

You can activate them directly, with no middleman:
1. Start your device
2. open settings and got to `Settings`>`General`>`About`
3. Click 7 times on the `Software Version Number` until you unlock the `Developer settings`
4. GO to `Developer Settings`, activate ADB there

## ADB on PC
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

ADB might already be installed if you developed Android apps, if you played around with custom ROMs or debloating Android devices. Check your command line for a `adb` command.

Otherwise, this process depends on your OS of choice

### Windows
1. Go to the [Official ADB Page](https://developer.android.com/studio/releases/platform-tools) and download the SDK Platform.
You can also use Android Studio, but for this it's overkill
2. Unpack the folder somewhere safe and without weird permission rules, C:/ADB/ is a classic
3. Add the folder to window's PATH variable, so you can just call `adb` via command line
   
### Linux
1. Some distros carry `adb` in their repo, check those to have the best experience
2. otherwise, you can go to the [Official ADB Page](https://developer.android.com/studio/releases/platform-tools) and download the SDK Platform.
2. Unpack the folder somewhere safe and without weird permission rules, HOME/ADB/ is probably fine
3. Add the folder to your home's `$PATH` variable or equivalent, so you can just call `adb` via command line

### macOS
We don't have any experience with macOS, so we can only point to what looks like an [exhaustive answer](https://stackoverflow.com/questions/17901692/set-up-adb-on-mac-os-x)

## Connecting ADB
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

Now that everything is set up, we can finally connect the devices:
1. boot both HMD and laptop up
2. connect via a USB cable
3. run `adb` on your laptop
4. check the HMD. A dialogue to enable USB debugging should appear. Allow it and set it to not ask again
5. check by calling `adb devices -l` on your PC, you should see a list of devices, including at least one

You're now connected!
This connection via cable is not comfortable, we can switch to Wi-Fi by calling 

```bash
adb tcpip 5555
adb connect <HMD_IP>:5555 
```
and disconnecting the USB cable upon success

This obviously requires your device to be on the same network as your PC

## ADB Commands
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

The basics use is installing LOVR and LODR, by calling 
```bash
adb install lovr.apk
```
and also updating your project by calling 
```bash
adb push --sync /path/to/project/. /sdcard/Android/data/org.lovr.app/files
```
Keep in mind if you're using LOVR or LODR, as they have different package names and therefore different folders:
 - LOVR: `org.lovr.app`
 - LODR: `org.lovr.hotswap`

Keep also in mind that remote files are not removed when removed locally, so you might have to clean up manually using `ls` and `rm`

ADB can do a lot, some useful resources are:
 - [Official ADB Docs](https://developer.android.com/studio/command-line/adb)
 - [ADB Cheat Sheet](https://www.automatetheplanet.com/wp-content/uploads/2019/08/Cheat_sheet_ADB.pdf)
 - [Oculus ADB Docs](https://developer.oculus.com/documentation/native/android/ts-adb/)
 - [My Tasks File](https://github.com/Udinanon/LOVR_Experiments/blob/main/.vscode/tasks.json)

I advise you to learn how to use the tasks functions or other similar methods of your IDE to make your workflow much smoother.

Some very useful commands include:

Capturing a screenshot and saving it on your PC to a folder with date and time in the name
```bash 
adb exec-out screencap -p > Screens/screen_$(date +'%Y-%m-%d-%X').png
```

Accessing remote `print` statements:
```bash 
adb logcat -s LOVR
```
  
Getting a slightly delayed HD screen stream
```bash
adb exec-out screenrecord --bit-rate=6m --output-format=h264 --size 1280x720 - | ffplay -framerate 24 -probesize 32 -sync video  -
```

Rebooting LOVR
```bash
adb shell am force-stop org.lovr.app
adb shell am start org.lovr.app/org.lovr.app.Activity
```

Accessing the remote shell of the HMD
```bash
adb shell
```

Doing it all at once
```bash
adb push --sync ${workspaceFolder}/Project/. /sdcard/Android/data/org.lovr.app/files
adb shell am force-stop org.lovr.app
adb shell am start org.lovr.app/org.lovr.app.Activity 
adb logcat -s LOVR
```