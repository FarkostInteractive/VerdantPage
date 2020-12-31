---
layout: default
title: "VerdantTerrain"
parent: "Component Reference"
nav_order: "2"
---

# VerdantTerrain

VerdantTerrain is the equivalent of [VerdantObject](VerdantObject.html) for Unity Terrains. By adding it you make Verdant aware of the terrain as a surface for vegetation. Just like Surface [VerdantObjects](VerdantObject.html) it can be masked by [VerdantObjects](VerdantObject.html) in mask mode.

Unlike [VerdantObject](VerdantObject.html) there are no mask textures. Rather, each type added to a VerdantTerrain is associated with one of the painted layers of the Unity Terrain. This means that if you have a grass layer and a rock layer Verdant can automatically fill in the grass layer with one of your types. If you just want to cover the entire terrain or place vegetation independently of its layers you can use a [VerdantObject](VerdantObject.html) in mask mode instead.

As you're working on yout terrain Verdant won't be able to know that it has changed. You can refresh it manually by pressing the Update Terrain button or automatically by checking Update Automatically. This will make Verdant refresh every single frame and can have a significant performance on complex scenes, though only when the terrain is open in the inspector.

When Verdant places instances onto the terrain it does so using its underlying heightmap. This can differ slightly from the mesh used to actually draw the terrain. If your vegetation is growing underground this is usually the reason, and you can solve it by increasing the resolution of your terrain.

If you are switching to Vulkan in the editor Unity has a bug that makes the terrain heightmap briefly unreadable. The result is that Verdant won't be able to place anything onto it. To fix it, simply make a small edit to the terrain and undo it. Your vegetation should return instantly and the bug won't bother you until you switch again.

## Parameters

|:---------------|:--------------------------|
| `Scale` | Sets the scale of vegetation for this terrain. Interacts with other scale parameters (eg. on the VerdantType) by multiplication. |
| `Max Slope` | The steepest slope in degrees onto which vegetation will be placed. Measured in world space along the Y axis. |
| `Edge Dithering` | Controls how much the terrain should dither the edges between different type layers. Can create the effect of different types smoothly dissolving into each other. Use the Debug Panel and check the type map to see the exact results |
| `Types` | The types of vegetation for this Terrain. Both VerdantType and VerdantGroup assets can be added here. Your scene can contain a maximum of 31 unique types or groups at any given time. |

## Public Methods

|:---------------|:--------------------------|
| `void AddType(VerdantInstantiable type)` | Adds a type into the type list and refreshes the scene. |
| `void RemoveType(VerdantInstantiable type)` | Removes a type from the type list and refreshes the scene. |
| `void RemoveTypeAt(int index)` | Removes the type at the specified index in the type list and refreshes the scene. |
| `void ClearTypes(int index)` | Removes all types from the terrain and refreshes the scene. |
| `int FindType(VerdantInstantiable type)` | Find the index of the given type in the type list. Returns -1 if it isn't found. |
| `VerdantInstantiable GetType(int index)` | Returns the type at the given index in the type list or throws if the index is out of range. |
| `VerdantInstantiable GetLayer(int index)` | Returns the type at the given index in the type list or throws if the index is out of range. |
| `void SetDirty()` | Marks this terrain as dirty and makes Verdant refresh the scene at the end of the frame. Needs to be called each time a parameter is changed to apply it. |


