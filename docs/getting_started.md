---
layout: default
parent: Docs
title: Getting Started
nav_order: 1
---
# Getting Started

{: .warning }
You should go and read the official [LOVR Guide](https://lovr.org/docs/Getting_Started).
It's much more detailed and thorough.

## PCVR
Just download LOVR from the website, extract, make a `main.lua` file in a folder and load the folder via LOVR.exe, such as `lovr.exe /path/to/project`.

You might want to setup your IDE for fast testing by setting up a built/run task with this.

## Android
You definitely want to activate ADB on your HMD, to more easily move files and stuff around. Setting it up Wirelessly is also probably a good idea. Refer to the [ADB](/docs/adb/) section of the guide for more info.

You'll also likely want to use [LODR](https://github.com/mcclure/lodr) for live reloading.

Download LOVR from the website, then install the APK to your device.
Crate a project folder on your PC, with a `main.lua` file. 
The folder needs to be sent to project folder on your HMD, which for the default app it's `/sdcard/Android/data/org.lovr.app/files`. This can be done via manually or via ADB with a `adb push --sync /local/folder/ /remote/folder/`. 

Keep in mind that LOVR does not restart when files are changed, you can either restart it manually, use ADB again or use [LODR](https://github.com/mcclure/lodr)