---
layout: default
parent: Docs
title: Lua
nav_order: 2
---
# Lua
The LOVR engine uses the Lua scripting language as the main methd to build programs. Almost everything is done in Lua, from importing models to defining objetcs, properties, funtions, players and so on. Major exclusions are shaders, done in OpenGL and lower level functions, possible via shared C libraries.

Lua is a high level, dynamic langauge. 
If you already have some basic experience with programming, Lua should be easy to pick up, with autmatic type inference, no complex types, small number of reserved keywords and high flexibility. 
You'll leikly have to build your own tools and methods to do a lot of non-trivial tasks, and you can also rely on a lot of available libraries built in Lua. 

The Lua version used in the engine is LuaJIT, so for more advaanced thing and some more complex libraries, keep an eye out.
## To get started
 - [Official Lua book](https://www.lua.org/pil/contents.html)
 - [Learn Lua in 15 minutes](https://tylerneylon.com/a/learn-lua/)
 - [Learn Lua in Y minutes](https://learnxinyminutes.com/docs/lua/)
 - [Lua on Exercism](https://exercism.org/docs/tracks/lua/resources)
 - [Lua syle guide](https://github.com/Olivine-Labs/lua-style-guide)

You can find more libraries on Github, but the LOVR docs have a [specific list](https://lovr.org/docs/Libraries) for tested or developed in house