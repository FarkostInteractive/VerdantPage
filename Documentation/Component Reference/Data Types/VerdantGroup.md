---
layout: default
title: "VerdantGroup"
parent: "Data Types"
grand_parent: "Component Reference"
---

# VerdantGroup

Represents a number of VerdantTypes that have been tied together to act as a single type. VerdantGroups are very useful for combining variants of a single plant or bundling different types that always appear together. 

A group can contain any number of VerdantTypes. This lets you get around the limitation of 31 active unique types per VerdantCamera as all the types in a group get treated counted as one type. *Be aware though that each type still adds some overhead*.

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