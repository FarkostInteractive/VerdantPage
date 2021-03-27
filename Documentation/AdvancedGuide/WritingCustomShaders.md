---
layout: default
title: "Writing Custom Shaders"
parent: "Advanced Guide"
---

# Writing Custom Shaders

It's very easy to extend Verdant with shaders of your own. You might want to fit it to a specific art style or strip out unneeded parts for performance reasons. As the Verdant Standard Shader is built for flexibility first you can potentially save a good bit of frame time by implementing one more tailored for your needs, while getting something with a look of its own. 

## Getting Started

To create a shader, simply go into the Create Asset dropdown menu and select Verdant > Shader. You'll find it below VerdantType and VerdantGroup. This will create a simple shader based on the unlit Unity shader adapted to include all the most important Verdant features.

## Using the Shader

To use the shader on your VerdantTypes you'll need to specify it in the Override Shader parameter. The shader you just created is ready to go as is, so select it and add the type to the scene. You'll see that it appears much simpler as it only applies the texture and color with no lighting.

## The Shader

If you have experience with shaders in Unity you'll probably find this one quite familiar. The additions are clearly delineated with comments explaining what they are and why Verdant needs them. As long as you respect everything outlined there you can work with the shader as you would any other.

## Shader Parameters

Custom shader parameters can be applied on a per-type basis by creating a regular Unity material and setting your shader on it. You can then set the material as the Override Material on VerdantType. The material will apply all of its properties before Verdant renders it and applies its parameters. It's important to note that Verdant cannot automatically detect changes in this material, so to see your changes in the editor you need to use the Reload Material button on the VerdantType. Properties on the material cannot be changed at runtime.

To access the per-LOD parameters on the VerdantType, please take a look at the table below which contains all their shader uniform names. As outlined there, some parameters are applied automatically and others you need to implement yourself. As a rule anything related to placement, like the Billboarding parameter, will be applied automatically, whereas surface features like the Normal Map must be added on a per-shader basis.  

|:---------------|:--------------------------|:------------|
| Parameter | Uniform | Applied by VerdantSetupInstancing or VerdantVertexProcessing |
| Color | `float4 verdantTypeParams_Color` | No |
| Texture | `float4 verdantTypeParams_Color` | No |
| Opacity Dithering | `float4 verdantTypeParams_Color` | No |
| Translucency Map | `float4 verdantTypeParams_Color` | No |
| Translucency | `float4 verdantTypeParams_Color` | No |
| Diffusion | `float4 verdantTypeParams_Color` | No |
| Normal Map | `float4 verdantTypeParams_Color` | No |
| Metallic | `float4 verdantTypeParams_Color` | No |
| Opacity Map | `float4 verdantTypeParams_Color` | No |
| Occlusion Map | `float4 verdantTypeParams_Color` | No |
| Billboarding | `float4 verdantTypeParams_Color` | No |
| Scale | `float4 verdantTypeParams_Color` | No |
| Deflection Angle | `float4 verdantTypeParams_Color` | No |
| Color Field Influence | `float4 verdantTypeParams_Color` | No |
| Scale Field Influence | `float4 verdantTypeParams_Color` | No |
| Stiffness | `float4 verdantTypeParams_Color` | No |
| Light Mode | `float4 verdantTypeParams_Color` | No |
| Ground Normals | `float4 verdantTypeParams_Color` | No |
| Fade Mode | `float4 verdantTypeParams_Color` | No |
| Receive Shadows | `float4 verdantTypeParams_Color` | No |
| Cast Shadows | `float4 verdantTypeParams_Color` | No |

## Deferred Shaders

Most of the time you will want to write your shaders as forward shaders, as those allow for much greater flexibility and are generally easier to work with. Unless you have experience both with both shaders and deferred renderering in Unity I'd advise against trying to write your own deferred one. If that doesn't discourage you, you'll have to look directly at how Verdant itself does it. 

Start by having a look at the file Shader_Verdant_Standard in Verdant > Runtime > Resources > Shaders. This is a Surface Shader which supports both forward and deferred rendering. It is by and large a typical Surface Shader but performs some additional work to pass along Verdant specific data like translucency.

You'll also want to look at the main deferred and deferred reflections shaders. If you are familiar with their standard Unity versions you'll be right at home in the Verdant versions, which only tweak them slightly. The file names are Verdant-DeferredShading and Verdant-DeferredReflections, and you'll find them under Verdant > Runtime > Resources > Shaders > Deferred.