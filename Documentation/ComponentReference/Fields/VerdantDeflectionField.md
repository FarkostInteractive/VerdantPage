---
layout: default
title: "VerdantDeflectionField"
parent: "Fields"
grand_parent: "Component Reference"
nav_order: "0"
---

# VerdantDeflectionField

Enables interactions that push or press vegetation by using [VerdantDeflectionAffectors](../Affectors/VerdantDeflectionAffector.html) on GameObjects. When deflected, vegetation will spring back to its original position using a physics simulation. 

VerdantDeflectionField is different from other fields in that it runs updates on a regular schedule. The Updates Per Second parameter controls the frequency of these updates. Verdant interpolates between frames, so no matter how low the number is motion will always apper smooth. What the update rate actually controls is how detailed the simulation will be and the interval at which affectors will influence the field. Generally, the smaller and faster your affectors are the more you will need to increase the number of updates, or the affectors might start leaving uneven trails. A lower number is always better for performance, so experiment to find a medium that works for your usage. 

Verdant simulates vegetation as a spring using [Hooke's Law](https://en.wikipedia.org/wiki/Hooke%27s_law). As it is bent, vegetation gains more energy the further it's pushed from its resting position. When it is released it snaps back, gaining speed as it is pulled. Because the velocity accumulates frame over frame it will most likely overshoot and bend to the opposite side, which in turn will cause an opposing force to pull it back again. Verdant models this using two 2D vectors for each pixel in the field.

The physics simulation can be a little bit tricky to control. It is highly dependent on the update rate, so try to find a good value for Updates Per Second first. Then, carefully read through what the other parameters do. It can also be useful to read up on Hooke's Law itself. That will make it much easier to understand how your changes affect the simulation. Changing them aimlessly can quickly put you in a spot where one parameter has an outsized influence and you lose track of what does what.

Also know that the field is normalized between zero and one and mainly controls *where* deflection happens. How much vegetation bends when deflected is determined by each [VerdantType](../Data Types/VerdantType.html) using the Deflection Angle parameter. This can have a huge impact on your visuals and is often the place to start rather than messing with the simulation.

For more information about fields in general, see the [Fields page](index.html). 

## Parameters
 
### Field Parameters

Changing any of the field parameters requires Verdant to replace the underlying render textures of the field. This can be very expensive and will almost certainly cause a frame hitch, so it should be avoided at runtime.

|:---------------|:--------------------------|
| `Resolution` | The resolution of the underlying field render textures, which along with range determines the level of detail per square meter the field can handle. The [Debug Panel](../../AdvancedGuide/DebugPanel.html) can be used to help visualize this. |
| `Range` | The render distance of the field. It determines how far the field should stretch around the camera. Outside of this range affectors will have no effect. |
| `Set Shader Values Globally` | When this is set, all the shader values used by Verdant will be made available as global shader variables. You need to enable it if you are using a Verdant shader on a regular material or if you want to read from this field in a custom shader. For details, check the page [Accessing Verdant Data]("../../AdvancedGuide/AccessingVerdantData.html") |

### Simulation Parameters

|:---------------|:--------------------------|
| `Updates Per Second` | The number of frames per second in the physical simulation. All of the following parameters will influence the simulation differently depending on update rate, so always expect to change them too after you've changed this.   |
| `Drag` | The amount of velocity lost over time. When drag is set to zero vegetation will continue to oscillate back and forth forever. As you increase it it will find its resting position quicker and oscillate less. |
| `Springiness` | Controls the strength of the "spring", or how strongly vegetation will rebound when it is released. Higher springiness will make vegetation harder to push down and it will oscillate more quickly when released. |
| `Timescale` | A simple multiplier for simulation time. If you're happy with the motion you have but want it slightly faster, this is the parameter to change. |


## Public Methods

|:---------------|:--------------------------|
| `void SetParameters()` | Applies any parameters that have been changed. This can be an expensive operation depending on what was changed. |


