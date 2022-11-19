---
layout: default
parent: Docs
title: Graphics
---

# Graphics

{: .warning}
This has heavily changed between version 0.15 and 0.16, with many functions changing, a completely different OpenGL version and a fundamentally different approach.
The following content covers version 0.15, for 0.16 see the [LOVR Docs](https://lovr.org/docs/v0.16.0/lovr.graphics)

Most of the graphical work in LOVR is made by the `lovr.graphics` module.
This allows us to draw simple primitives, load and manipulate complex objects such as meshes or joints, apply geometric transforms on bodies, manipulate the window in PCVR and run shaders.

Methods to draw primitives, such as cubes or skyboxes, are called directly at the `lovr.draw()` step, so at each frame
``` lua
function lovr.draw()

    local position = vec3(lovr.headset.getPosition("right")) -- read a vector from the positon of the right hand
    lovr.graphics.sphere(position, .01) -- generate a 1cm radius spehere at the position

end
```
Vectors are automatically unpacked into single arguments, making code much easier to read

To draw more complex objects, such as those created in 3D modelling software we need to import them from files, usually done at load time in `lovr.load()`, and then each object has a `:draw()` call to be executed at each frame
```lua
function lovr.load()
    corridor = lovr.graphics.newModel("Assets/corridor.obj") -- import model from file
end

function lovr.draw()
    corridor:draw(0,-2, 0)
end
```
Support file types and more details can be found on the [specific Docs page](https://lovr.org/docs/v0.15.0/Model)

You might notice that objects look odd, either completely flat or completely black. This is because LOVR by default uses a very simple shader that simulates no light and uses flat colors.
To get better results you'll need to either try the other [included shaders](https://lovr.org/docs/v0.15.0/DefaultShader), by calling `default_shader = lovr.graphics.newShader(default, options)` and then `lovr.graphics.setShader(default_shader)`, or you'll have to learn something about shaders, OpenGL and how game graphics work. 

## Shaders
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

Shaders are GPU software that computes 3D information to create 2D images. They get the position of models, textures, lights, effects, particles and so on and return the color values of pixels. 
Shaders can be an extremely complex topic, fundamental for 3D rendering, and can be used also for parallel high performance computations, in which case they're called Compute Shaders.

To read some introductory material on shaders:
 - [AeroTwist Intro to shaders](https://aerotwist.com/tutorials/an-introduction-to-shaders-part-1/)
 - [The Book of shaders](https://thebookofshaders.com/)
 - [What Are Shaders?](https://www.youtube.com/watch?v=sXbdF4KjNOc)
 - [How Lighting (Basically) works in Games](https://www.youtube.com/watch?v=VXggMZvqSvM)
 - [Shader Basics](https://www.youtube.com/watch?v=UVNnkDqcTGE)
 - [LOVR 0.16 Shaders docs](https://lovr.org/docs/v0.16.0/Shaders)

More advanced material can be found at 
 - [OpenGL shading docs](https://www.khronos.org/files/opengles_shading_language.pdf)
 - [Shader Toy](https://www.shadertoy.com/)

Don't be afraid to experiment and test, shaders can be complicated, but even basic functions and some creativity can give you a lot of great results. 
Trying random code from the internet is also highly encouraged

LOVR shaders can communicate with the main Lua program by two means: 
 - `uniforms <type> <name>` values, passed to the GPU with `shader:send(<name>, <value>)`, such as positions in 3D or other simple data
 - `ShaderBlocks` which can pass much more data easily, such as a table of objects and more

For more info, make reference to the [New Shader Block Docs](https://lovr.org/docs/v0.15.0/lovr.graphics.newShaderBlock) and [Shader Block Docs](https://lovr.org/docs/v0.15.0/ShaderBlock)

A correct usage of shaders and other functions can be used to completely overhaul the 3D rendering process, allowing you to draw images with methods such as [Ray Marching](https://www.youtube.com/watch?v=PGtv-dBi2wE).

### Vertex Shaders
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

These shaders compute the 3D geometrical properties of the model, having access to parameters such as vertex position, transform matrices for the view camera, the projection matrix and more.
The returned value is the position in the world of the vertex after being processed.

The default is 
```glsl
    vec4 position(mat4 projection, mat4 transform, vec4 vertex) {
    return vertex;
    }
```
which does fundamentally nothing to the vertex

Values here can be exfiltrated to the Fragment shader by declaring a `out <type> <name>` variable and defining them in the shader code.

Some of the available default values are 
```glsl 
in vec3 lovrPosition; // The vertex position in meters, relative to the model itself
in vec3 lovrNormal; // The vertex normal vector
in vec2 lovrTexCoord;
in vec4 lovrVertexColor;
in vec3 lovrTangent;
in uvec4 lovrBones;
in vec4 lovrBoneWeights;
in uint lovrDrawID;
out vec4 lovrGraphicsColor;
uniform mat4 lovrModel; // 4x4 matrix with model world coords and rotation
uniform mat4 lovrView;
uniform mat4 lovrProjection;
uniform mat4 lovrTransform; // Model-View matrix
uniform mat3 lovrNormalMatrix; // Inverse-transpose of lovrModel
uniform mat3 lovrMaterialTransform;
uniform float lovrPointSize;
uniform mat4 lovrPose[48];
uniform int lovrViewportCount;
uniform int lovrViewID;
const mat4 lovrPoseMatrix; // Bone-weighted pose
const int lovrInstanceID; // Current instance ID
```

### Fragment Shaders
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

These shaders compute the final color of the pixel, or "fragment".
They are called after the geometry shader.

This is the default fragment shader
```glsl
vec4 color(vec4 graphicsColor, sampler2D image, vec2 uv) {
  return graphicsColor * lovrDiffuseColor * lovrVertexColor * texture(image, uv);
}
```
With `uv` being the 2D coordinates of the face being rendered, normalized in a [0.0 1.0] range. This shader integrates parameters such as the texture of the object, the color set in `lovr.graphics.setColor()`, diffuse color and a few more, but no lighting.

We can access shared values from the Vertex Shader with `in <type> <name>`

The standard header is 
```glsl
in vec2 lovrTexCoord;
in vec4 lovrVertexColor;
in vec4 lovrGraphicsColor;
out vec4 lovrCanvas[gl_MaxDrawBuffers];
uniform float lovrMetalness;
uniform float lovrRoughness;
uniform vec4 lovrDiffuseColor;
uniform vec4 lovrEmissiveColor;
uniform sampler2D lovrDiffuseTexture;
uniform sampler2D lovrEmissiveTexture;
uniform sampler2D lovrMetalnessTexture;
uniform sampler2D lovrRoughnessTexture;
uniform sampler2D lovrOcclusionTexture;
uniform sampler2D lovrNormalTexture;
uniform samplerCube lovrEnvironmentTexture;
uniform int lovrViewportCount;
uniform int lovrViewID;
```

### Compute Shaders
--------------------------------------------------------------------------------------------------------------
{: .mt-2 .mb-4}

These are not graphical shaders, but programs run on the GPU to harness its highly parallel architecture. They usually rely on much more data being moved to and from the GPU and require a deeper understanding of GPU hardware and parallel programming methods and challenges to be used effectively, but can also be extremely useful and powerful. 
