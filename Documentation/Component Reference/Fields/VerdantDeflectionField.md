---
layout: default
title: "VerdantDeflectionField"
parent: "Fields"
grand_parent: "Component Reference"
nav_order: "0"
---

# VerdantDeflectionField

A component that makes it possible to interact with vegetation around the camera by pushing or pressing it. 

Objects that want to interact with this field need to have [VerdantDeflectionAffector](../Affectors/VerdantDeflectionAffector.html) attached.

For more information about fields in general, see the [Fields page](index.html). 

## Parameters
 
### Field Parameters

Changing any of the field parameters requires Verdant to replace the underlying render textures of the field. This can be very expensive and will almost certainly cause a frame hitch, so it should be avoided at runtime.

|:---------------|:--------------------------|
| `Resolution` | The resolution of the underlying field render textures, which along with range determines the level of detail per square meter the field can handle. The [Debug Panel](../../UserGuide/DebugPanel.html) can be used to help visualize this. |
| `Range` | Can be thought of as the render distance of fields. It determines how far the field should stretch around the camera. Outside of this range affectors will not be able to influence it. |
| `Set Shader Values Globally` | When this is set, all the shader values that Verdant uses will be made available as global shader variables. You need to enable it if you are using a Verdant shader on a regular material or if you want to read from this field in a custom shader. For details, check the page [Writing Custom Shaders]("../../UserGuide/WritingCustomShaders.html") |

### Simulation Parameters

Verdant simulates vegetation as a spring using [Hooke's Law](https://en.wikipedia.org/wiki/Hooke%27s_law). The short explanation is that as it is bent, vegetation gains more energy the further it bends from its resting position. When it is released it snaps back, gaining speed as it is pulled. Because the velocity accumulates frame over frame it will most likely overshoot and bend to the opposite side, which in turn will cause an opposing force to pull it back again. This behaviour is very intuitive, but the parameters controlling it are unfortunately less so.  

|:---------------|:--------------------------|
| `Updates Per Second` | The number of frames per second in the physical simulation. Verdant interpolates between frames, so no matter how low the number is motion will always apper smooth. What this parameter actually controls is how detailed the simulation will be and the interval at which affectors will influence the field. Generally, as your affectors become smaller and faster you will need to increase the number of updates, or they might start leaving uneven trails. A lower number is always better for performance though, so you will have to experiment to find a medium that works for your usage. All of the following parameters will influence the simulation differently depending on update rate, so always expect to change them too after you've changed this.   |
| `Drag` | The amount of velocity lost over time. When drag is set to zero vegetation will continue to oscillate back and forth forever. As you increase it it will find its resting position quicker and oscillate less. |
| `Springiness` | Controls the strength of the "spring", or how strongly vegetation will rebound when it is released. Higher springiness will make vegetation harder to push down and it will oscillate more quickly when released. |
| `Timescale` | A simple multiplier for simulation time. If you're happy with the motion you have but want it slightly faster, this is the parameter to change. |

Furthermore, [VerdantType](../Data Types/VerdantType.html) has a parameter called Deflection Angle that controls how many degrees deflection will bend that particular type. While all vegetation shares the same normalized simulation, it's possible to have one plant only bend ten degrees and another a full 90. This can have a huge impact on how the motion looks, so it's worth experimenting with that and not just the simulation parameters.


## Public Methods

|:---------------|:--------------------------|
| `void SetParameters()` | Applies any parameters that have been changed. This can be an expensive operation depending on what was changed. |


