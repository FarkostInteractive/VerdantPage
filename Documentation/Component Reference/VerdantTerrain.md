---
layout: default
title: "VerdantTerrain"
parent: "Component Reference"
nav_order: "2"
---

# VerdantTerrain

*******

Because Verdant can't know when the terrain has changed you need to either update it manually by pressing the Update Terrain button or automatically by checking Update Automatically. This will make Verdant refresh every single frame, which can be quite demanding depending on how complex your scene is.

## Parameters

|:---------------|:--------------------------|
| `Scale` | Sets the scale of vegetation for this terrain. Interacts with other scale parameters (eg. on the VerdantType) by multiplication. |
| `Max Slope` | The steepest slope in degrees onto which vegetation will be placed. Measured in world space along the Y axis. |
| `Edge Dithering` | Controls how much the terrain should dither the edges between different type layers. Can create the effect of different types smoothly dissolving into each other. Use the Debug Panel and check the type map to see the exact results |
| `Types` | The types of vegetation for this Terrain. Both VerdantType and VerdantGroup assets can be added here. Your scene can contain a maximum of 31 unique types or groups at any given time. |

## Public Methods

|:---------------|:--------------------------|
| `void AddType(VerdantInstantiable type)`{: .csharp} | Adds a type into the type list and refreshes the scene. |
| `void RemoveType(VerdantInstantiable type)` | Removes a type from the type list and refreshes the scene. |
| `void RemoveTypeAt(int index)` | Removes the type at the specified index in the type list and refreshes the scene. |
| `void ClearTypes(int index)` | Removes all types from the terrain and refreshes the scene. |
| `int FindType(VerdantInstantiable type)` | Find the index of the given type in the type list. Returns -1 if it isn't found. |
| `VerdantInstantiable GetType(int index)` | Returns the type at the given index in the type list or throws if the index is out of range. |
| `VerdantInstantiable GetLayer(int index)` | Returns the type at the given index in the type list or throws if the index is out of range. |
| `void SetDirty()` | Marks this terrain as dirty and makes Verdant refresh the scene at the end of the frame. Needs to be called each time a parameter is changed to apply it. |


