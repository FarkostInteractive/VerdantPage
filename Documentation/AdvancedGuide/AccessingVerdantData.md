---
layout: default
title: "Accessing Verdant Data"
parent: "Advanced Guide"
---

# Accessing Verdant Data

It can sometimes be useful to access the data Verdant produces in another shader. You might want to use the wind to animate something or to apply cloud shadows on your terrain. Most of these values can be exposed globally, where they can then be picked up by any material using parts of the Verdant shader library.

## Global Shader Data

The first step to accessing a system is to make sure its data is available to your shaders. For all the sampling functions we'll cover here there's a checkbox either on the field component or on VerdantCamera called something to the effect of "Set Shader Values Globally". You'll need to tick it for each system you want to access. Note that the data being global implies that these features only work on one VerdantCamera at a time. If you have the checkboxes set on more than one you'll see some flickering as the two fight for the global shader parameters. A common case where this might happen is when rendering in scene view while having the game view open.

## Wind

If you've tried to use global wind you'll have already seen that it's a bit different from other global data. Wind is calculated per VerdantType, so when using it globally we need to select a template type to base our data on. That VerdantType *must* be active in the scene, or it won't be usable as a template. To allow for different wind configurations for different materials you can also specify a material-VerdantType pairing. The material will have the wind of that VerdantType applied to it directly. Aside from needing to specify every material you want to use, for the purposes of this guide it works the same way as the global parameters do. 

## Including the Access Functions

Verdant offers convenient functions for sampling data from its various systems. To use them you must include their corresponding files. Select the ones you need, or use 'All Fields' to include access to all of them. The files and their include paths are listed below:

|:---------------|:--------------------------|
| All Fields | `#include "Packages/com.farkostinteractive.verdant/Runtime/Resources/Shaders/Includes/VerdantFieldSampling.cginc"` |
| Color Field | `#include "Packages/com.farkostinteractive.verdant/Runtime/Resources/Shaders/Includes/FieldSampling.VerdantColorFieldSampling.cginc"` |
| Scale Field | `#include "Packages/com.farkostinteractive.verdant/Runtime/Resources/Shaders/Includes/FieldSampling.VerdantScaleFieldSampling.cginc"` |
| Deflection Field | `#include "Packages/com.farkostinteractive.verdant/Runtime/Resources/Shaders/Includes/FieldSampling.VerdantDeflectionFieldSampling.cginc"` |
| Height Field | `#include "Packages/com.farkostinteractive.verdant/Runtime/Resources/Shaders/Includes/FieldSampling.VerdantHeightFieldSampling.cginc"` |
| Cloud Shadows | `#include "Packages/com.farkostinteractive.verdant/Runtime/Resources/Shaders/Includes/FieldSampling.VerdantCloudShadowSampling.cginc"` |
| Wind | `#include "Packages/com.farkostinteractive.verdant/Runtime/Resources/Shaders/Includes/FieldSampling.VerdantWindSampling.cginc"` |

## Function Prerequisites

Verdant relies heavily on structured buffers and compute shaders, so any shader accessing its systems must have `#pragma target 4.5`. Without it the shader compiler will attempt to compile it for hardware that can't support those features and the access functions will be stripped out. For the affector fields you also need to include the corresponing multi_compile pragma. This allows the shader to behave properly even if that field is disabled or unavailable:

|:---------------|:--------------------------|
| Color Field | `#pragma multi_compile _ VERDANT_COLORFIELD_ON` |
| Scale Field | `#pragma multi_compile _ VERDANT_SCALEFIELD_ON` |
| Deflection Field | `#pragma multi_compile _ VERDANT_DEFLECTIONFIELD_ON` |

## Using the Functions

With the prerequisites out of the way we are ready to start accessing! Most of the access functions follow a similar pattern: You supply a world space position and the function returns the data at that location. 

### Color Field
|:---------------|:--------------------------|
| `float4 VerdantSampleColorField (float4 wPos)` |

Takes the world position and returns the color value of the field at that location.

### Scale Field
|:---------------|:--------------------------|
| `float VerdantSampleScaleField (float4 wPos)` |

Takes the world position and returns the scale value of the field at that location.

### Deflection Field
|:---------------|:--------------------------|
| `float4 VerdantSampleDeflectionField (float4 wPos)` |

Takes the world position and returns the deflection vector and deflection velodity vector at that location, both of which are 2D vectors. The main vector is stored in XY and the velocity in ZW. Both are to be interpreted in the range 0-1, though the main vector can be longer, in which case it represents the press time where the amount of seconds left is the magnitude-1. Normalze it if that's the case.

### Height Field
|:---------------|:--------------------------|
| `float4 VerdantSampleCoarseHeightFieldSmooth (float4 position)` |
| `float4 VerdantSampleDetailHeightFieldSmooth (float4 position)` |
| `float4 VerdantSampleCoarseHeightFieldSharp (float4 position)` |
| `float4 VerdantSampleDetailHeightFieldSharp (float4 position)` |
| `float4 VerdantSampleCoarseHeightFieldHybrid (float4 position, float smoothing)` |
| `float4 VerdantSampleDetailHeightFieldHybrid (float4 position, float smoothing)` |
| `bool VerdantInCoarseHeightField(float4 wPos)` |
| `bool VerdantInDetailHeightField(float4 wPos)` |

The height field is special, as it actually consists of two different fields and can be sampled in a variety of ways. The Coarse-type functions access the bigger field that spans the entire render distance, and the Detail-types access the denser field that spans the detail render distance. You can see a visualization of the two by opening the Precision Settings menu on your VerdantCamera. You can select between them easily by using VerdantInCoarseHeightField and VerdantInDetailHeightField. Do note that the coarse height field encompasses the detail height field, so use that function to check if either is available and then the detail function to select between them.

The Smooth, Sharp and Hybrid distinctions map to the Placement Modes you can set on VerdantCamera. They represent different ways of sampling the field, Sharp being just the pure pixels, Smooth being bilinearly interpolated and Hybrid being a mix that uses Smooth for even surfaces and Sharp for sudden changes, preserving edges but having a sligtly higher performance cost. Hybrid also has a smoothing parameter that matches Smoothing Level on VerdantCamera. Which one to use depends on your needs, though for most cases where precision isn't a high priority either Sharp and Smooth will do just fine and save you some frame time compared to Hybrid.

All functions return the data as follows: The X component contains the height in world space, the Y component contains the scale, and ZW contain the X and Z components of the heightfield world space normal which can be reconstructed using cross multiplication.

### Cloud Shadows
|:-----------------------------------------|
| `float4 VerdantSampleCloudShadows(float4 wPos)` |

Takes the world position and returns the cloud shadow texture at that location.

### Wind
|:-----------------------------------------|
| `float VerdantSampleWind (float4 wPos, half3 normal, float noise, float strength, float height)` |
| `float VerdantSampleWindNoise (float4 wPos)` |
| `float3 VerdantWindDirection()` |

It should be immediately obvious that wind is a little more complicated than the other functions. That's because wind is much more closely tied to Verdant instances, being influenced both by a noise based on the unique ID of each instance and their procedurally calculated rotation. As non-Verdant shaders have access to neither we have to supply other substitute data.

For the noise you can input any value that is in the range 0-1 and consistent over every vertex. Normal needs to be similarly consistent and is used to approximate how much wind from a given direction the mesh is catching. For meshes with many different parts responding individually to wind, like the leaves on a tree, it's important that each piece has consistent noise and normal values across all of it. If the piece is flat you can simply use the surface normal, and the noise can be sampled from a special texture with a random 0-1 value painted onto each piece (or any variety of shader-based methods that produce similar results). Height is only necessary for things that bend, like blades of grass, where you can enter the object space Y value. For other things, just set it to zero. Strength is simply a 0-1 ranged float that acts as a throttle on the total amount of wind. You'll usually want to leave it to 1. The return value is given in degrees from 0 to 90.

If you are only interested in the broad strokes, call VerdantSampleWindNoise. That function will behave similarly to the others and simply return the wind noise at the given position. Its return value is a float between 0 and 1 representint the strength of the wind.

Finally, VerdantWindDirection gives the direction of the wind in world space. You'll need it to apply the result of VerdantSampleWind correctly. The movement of the wind noise already takes it into account.