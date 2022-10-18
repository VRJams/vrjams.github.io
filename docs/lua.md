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
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

These are some good resources, both for complete novices and more experienced programmers.

 - [Official Lua book](https://www.lua.org/pil/contents.html)
 - [Great Lua Guide](https://docs.otland.net/lua-guide/)
 - [Lua Users Wiki](https://lua-users.org/wiki/)
 - [Learn Lua in 15 minutes](https://tylerneylon.com/a/learn-lua/)
 - [Learn Lua in Y minutes](https://learnxinyminutes.com/docs/lua/)
 - [Lua on Exercism](https://exercism.org/docs/tracks/lua/resources)
 - [Lua syle guide](https://github.com/Olivine-Labs/lua-style-guide)

If you prefer learning by example, you should visit:
 - [LOVR Slack](https://lovr.org/slack) - has a nice Showcase channel
 - [My experiments](https://github.com/Udinanon/LOVR_Experiments) - see how we (barely) make it work


## Libraries and Object Oriented Programming
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

LOVR doesn't have to run on a single `main` file; while this is the core of the program it can also call other files, via Lua's `require`.

As the [LOVR Docs](https://lovr.org/docs/v0.15.0/Libraries) show, you can call other files via this function and import their functionality, allowing you to create libraries and use them between projects and sharing them with others.

```lua
-- library.lua is sitting next to our main.lua here
local lib = require('library')

function lovr.load()
  lib.doStuff()
end
```

The LOVR docs have a [list](https://lovr.org/docs/Libraries) for some teter with LOVR or developed with LOVR in mind directly.

One of the main features this allows us to do is exend the core libraries of LUa, which are very small compared to languages like Python. This includes

### Object Oriented Programming
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

No, Lua does not natively use OOP. It can be implemented easily using Lua's tables
There are two ways:
1. use a library like [30log](https://github.com/Yonaba/30log) or [classic](https://github.com/rxi/classic)
2. make your own Objects and Classes

Making your own is a good exercise to understand better Lua tables and metatables

Some resources for going deeper on the topic are:
 - [Lua Guide section on metatables](https://docs.otland.net/lua-guide/concepts/metatables)
 - [Lua Object Oriented Programming](https://www.lua.org/pil/16.htmlù)
 - [Lua User Wiki about OOP](https://lua-users.org/wiki/ObjectOrientedProgramming)
 - [Lua Book on `_index` metatable](http://www.lua.org/pil/13.4.1.html)

Or you can use these examples from [My Notes](https://github.com/Udinanon/LOVR_Experiments/blob/main/notes.md):

#### Objects
Tables are the fundamental associative arrays of Lua. These can be used to build dictionaries, lists, vectors, arrays and objects.

Simply define a function as

```lua
obj={}
function obj.method(self)
    -- do stuff
end
```
and you have a valid object. 

#### Classes
Here we need the metatables. These are advanced tables that can define more complex properties such as interation through operatores like `+` or `==`, but also thngs like length, calling the table like a function, what to do when a certain value is called and what to do whena  new value is defined.

The core question is defining standard keys and values for standard functions, those of the class, so each instance can call the same functions but with their values.
This function is covered by the `__index` metatable, which defines where else to go to check for unkown indeces

This is acheived by defining the functions for the class on an empty or minimal table, then at initialization of the instance, we initiaize a new table with the needed values, asssociate the insatne wth the class via the `__index` metatable and returning the insatnce. 
Each instance will access the methods of the class unless overridden and any usage of `self` will be in reference to the instance, not the class

```lua
Class = {}  -- empty table used by class

function Class.new(self, val1, val_2) --define generting method
    local instance = {
        key_1 = val_1,
        key_2 = val_2
    } -- crate table for instance and fill with datas
    setmetatable(instance, { __index = Class }) --associate the instance with the class object and inherit the methods and properties
    return instance -- return the instance, not the class
end
```

