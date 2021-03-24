---
layout: default
title: "VerdantType Parameters"
parent: "Shaders"
grand_parent: "Component Reference"
nav_order: "1"
---

# VerdantType Parameters
The uniform names listed here map to the per-LOD properties on VerdantType, all of which can be accessed in your custom shaders. 

## Parameters not used in VerdantShadingFunctions
These are properties for which you will always need to add the uniform declaration and implementation to the shader yourself. Once declared the uniform value will be set automatically. 

|:---------------|:--------------------------|
| `Color` | The texture to use. |
| `Texture` | Sets how often the texture should repeat in world X and Z throughout the field. |
| `Normal Map` | When set the texture will be interpreted as a greyscale texture where black maps to Minimum. |
| `Translucency` | The minimum color used for a remapped texture. |
| `Occlusion` | The maximum color used for a remapped texture. |

## Parameters used in VerdantShadingFunctions
These are properties which you can simply access by name as long as VerdantShaderFunctions.cginc is included in the shader. If you also use VerdantSetupInstancing and VerdantVertexProcessing they will be applied automatically, and you'll only need to use them if you want to add custom behaviour. If VerdantshaderFunctions is not included, you'll have to declare them yourself just like the other parameters.

|:---------------|:--------------------------|
| `Billboarding` | The texture to use. |
