---
layout: default
title: "VerdantGroup"
parent: "Data Types"
grand_parent: "Component Reference"
---

# VerdantGroup

VerdantGroups are a convenient way to bundle up several different VerdantTypes. They can be used wherever VerdantTypes are and have the useful ability to get around the limit of 31 active types per scene. While a group can contain any number of types they are counted as if they were a single VerdantType in the scene. Whenever you find yourself using multiple types together consistently, like several variants of one plant or different types of grass that often grow in the same field, you should bundling them into a group.

Groups have their own parameters for density and falloff. Each type also has a ratio parameter that determines how much of the density is allocated to this type. When a type is first added it will calculate a ratio based on its own density and the density of the group, eg. a 100 density type in a 100 density group will have ratio 1, a 50 density type will have 0.5, etc. While you can set the ratios as high as you like it's usually good practice to ensure they add up to 1. That way you know that the total density won't exceed that of the group.  

As groups still allow you to fully configure each type separately they do incur a performance overhead for each new type. Even though it's theoretically possible to add as many types as you like you should always be mindful of how your scene runs as you do so. This is especially true if you aren't balancing your densities, in which case you might find yourself accidentally drawing multiple the instance count you intended to. 

## Parameters

|:---------------|:--------------------------|
| `Density` | An override of the density of all the VerdantTypes in the group. |

### Falloff

As with Density, groups have a single falloff that overrides all the falloffs of their types.

|:---------------|:--------------------------|
| `Mode` | Lets you select one of three preset falloff modes or Custom, which can be adjusted manually. Which mode to use depends on how close the camera is to the ground. In most cases Sharp or Exponential will be best. Linear makes the amount of vegetation much higher but can work well for aerial views with a smaller render distance. In general, always try to get the density down as low as possible as close to the camera as possible.  |

### Types

|:---------------|:--------------------------|
| `Type` | A type in the group |
| `Ratio` | A multiplier on the density of this type |