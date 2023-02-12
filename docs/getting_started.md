---
layout: default
parent: Docs
title: Setting Up
nav_order: 1
---
# Setting up LÖVR

{: .warning }
For this section, you should also go and read the official [LOVR Guide](https://lovr.org/docs/Getting_Started).
It's much more detailed and thorough.

## PCVR
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

Download LOVR from the [website](https://lovr.org/downloads) and extract it to a useful folder, such as `/Engine/`.

Next, create a project folder, such as `/Project/`. Here all you code, assets and other files will reside. For now, just make a `main.lua` file with the following code:
```lua
function lovr.draw()
  lovr.graphics.print('hello world', 0, 1.7, -3, .5)
end
```

Last, load the folder via `LOVR.exe`, such as `/Engine/lovr.exe /Project/`.

Remember to load the folder and not the Lua file
{: .warning }

You should see the Hello World message

## Android
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

You'll need to have your device in developer mode or equivalent, so for the Quest you can see [their own guide](https://developer.oculus.com/documentation/quest/latest/concepts/mobile-device-setup-quest/) or any other equivalent, the key needs are being able to access files, install applications and use `adb`. 
Setting up `WiFi ADB` is also probably a good idea. Refer to the [ADB](/docs/adb/) section of the guide for more info.

You'll also likely want to use [LODR](https://github.com/mcclure/lodr) for live reloading.

Download LOVR from the [website](https://lovr.org/downloads), then install the APK to your device.

Next, create a project folder, such as `/Project/`. Here all you code, assets and other files will reside. For now, just make a `main.lua` file with the following code:
```lua
function lovr.draw()
  lovr.graphics.print('hello world', 0, 1.7, -3, .5)
end
```

The folder needs to be sent to a folder on your HMD, which for the default app is `/sdcard/Android/data/org.lovr.app/files`.

This can be done via manually or via ADB with `adb push --sync /local/folder/to/Project/. /remote/folder/`. 

Keep in mind that LOVR does not restart when files are changed, you can either restart it manually, use ADB again or use [LODR](https://github.com/mcclure/lodr)

You should see the "Hello World" message!

# Setting up your IDE
My only experience is with VSCode, which I recommend, but writing Lua and executing LÖVR does not require any special setup, so anything you want is fine.

## VSCode
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

Having a couple of extensions for Lua debugging makes it easier to spot errors beforehand and gives highlighting. I use:
 - [Lua](https://marketplace.visualstudio.com/items?itemName=keyring.Lua)
 - [Lua Language Server](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)
 - [Lua Debug](https://marketplace.visualstudio.com/items?itemName=actboy168.lua-debug)

You can then set up these to support LOVR in detail using the [official guide](https://gist.github.com/ussaohelcim/9eca6eaa903eefff07b4f3e2019de915), this gives you access to documentation about the functions and methods directly in the IDE.

You might also want to set up quick Tasks to run the Project, be it via ADB for mobile platforms or local shell. Furthermore, you can find my setup at my [Repo](https://github.com/Udinanon/LOVR_Experiments/blob/main/.vscode/tasks.json), for Quest devices
