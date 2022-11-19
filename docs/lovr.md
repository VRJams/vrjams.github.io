---
layout: default
parent: Docs
title: LÖVR
nav_order: 3
---

# LÖVR

The star of the show, LÖVR is an open source framework to create VR applications easily. It's built in C, with scripting done using LuaJIT to execute Lua code on the fly. It supports multiple HMD models, including both PCVR and Android based platforms, it's extremely fast and responsive, with an active community and development team.

The project has an [Official Website](https://LoVR.org/), with links to the [Documentation](https://LoVR.org/docs), the [GitHub](https://github.com/bjornbytes/lovr) for the code, a [Matrix room](https://matrix.to/#/!XVAslexgYDYQnYnZBP:matrix.org) and [Slack group](https://lovr.org/slack) to talk with the devs.

The entire project is maintained under the MIT license.

## How to use LÖVR
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

{: .warning}
LOVR has very recently dropped its new 0.16 version, which has a lot of changes, both under the hood and for the user. 
The instructions here reference the older 0.15 version and its mechanisms. Be sure to use the correct docs on the [LOVR Page](https://lovr.org/docs/)

More information can be found at the [Official Docs](https://lovr.org/docs/v0.15.0/Callbacks_and_Modules)

After installing the framework on your device (see [the docs](getting_started.md)), you will have a folder, such as `/Project/`, with a `main.lua` file. This file is the one to be executed by LÖVR. 

LOVR executes the `main` file by executing the callbacks defined in it, like `lovr.draw()`.
These callbacks are functions that LÖVR calls at specific moments. The most notable are:
 - `lovr.load()`, executed at the start of the program
 - `lovr.update(dt)`, used for physics and game logic, passing also the `dt` value of the delta time since the last call
 - `lovr.draw()`, called at each frame to generate the image

LOVR also exposes a series of modules to execute functions, such as the `lovr.headset` module which gives access to things like headset and hands positions, number of tracked limbs and so on, or the `lovr.graphics` which gives us functions to draw models, textures and more.