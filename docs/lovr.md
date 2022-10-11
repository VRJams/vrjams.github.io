---
layout: default
parent: Docs
title: LÖVR
nav_order: 3
---

# LÖVR

THe star of the show, LÖVR is a open source frameowrk to create VR applications easily. It's built in C, with wcripting done using LuaJIT to execute Lua code on the fly, it support multiple HMD models, including both PCVR and Adnroid based platforms, it's extremely fast and responsive, with an active community and development team.

The project has an [Official Website](https://lovr.org/), with links to the [Documentation](https://lovr.org/docs), the [Github](https://github.com/bjornbytes/lovr) for the code, a [Matrix room](https://matrix.to/#/!XVAslexgYDYQnYnZBP:matrix.org) and [Slack group](https://lovr.org/slack) to talk with the devs.
The entire project is mantained under the MIT license.

## How to use LÖVR
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

After instlling the framework on you device (see [the docs](getting_started.md)), you will have a a folder, such as `/Project/`, with a `main.lua` file. This file is the one to be executed by LOVR. 

LOVR executes the `main` file by executing the callbacks definied in it, like `lovr.draw()`.
These callbacks are functions that LOVR calls at specific moments. THe most notable are:
 - `lovr.draw()`, called at each frame to generate the image
 - `lovr.update(dt)`, used for physiscs and game logic, passing also the `dt` value of the delta time since the last call
 - `lovr.load()`, executed at the start of the program

LOVR also exposes a seres of modues to execute functions, such as the `lovr.headset` module which gives access to thinsg ike headset and hands positions, number of tracke limbs and so on, or the `lovr.graphics` which gives us functions to draw models, textures and more.

More information can be found at the [Official Docs](https://lovr.org/docs/v0.15.0/Callbacks_and_Modules)