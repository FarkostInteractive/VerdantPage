---
layout: default
title: "VerdantObject"
parent: "Component Reference"
---


# VerdantObject
Determines where in the scene vegetation should be placed. This component is used with Mesh Renderer and/or Verdant Shape Descriptor. If you want to use Verdant with terrains you have to use the similar component VerdantTerrain instead. 

VerdantObject has two modes: Surface and Mask. 
Surface objects place vegetation onto themselves. Generally you will add a VerdantObject to a GameObject that has a Mesh Renderer to make Verdant aware of it.  

Mask objects on the other hand influence the vegetation on Surface objects below them. A mask can multiply the scale of the surface below, add more types, or do a combination of both.

For both modes you will also need to specify which types you want the VerdantObject to have. Both VerdantTypes and VerdantGroups are allowed. 

You can control where vegetation grows in more detail by using a mask texture. You can either use a greyscale texture or paint a mask directly by using the built in paint tools. 

## Painting a Texture Mask
You can find the tools for texture mask painting either in the VerdantObject inspector or in the Scene View Tools shelf (can be enabled or disabled with the tool icon in the upper right corner). When you first use them you will be told to initialize the texture mask. Doing so will create a new data structure that is serialized along with the component. 

## Parameters

|:---------------|:--------------------------|
| `Mode` | Determines whether vegetation should be placed onto this object (Surface) or onto the objects below it (Mask). |
| `Scale` | Sets the scale of vegetation for this object. Interacts with other scale parameters (eg. on the VerdantType) by multiplication. |
| `Max Slope` | The steepest slope in degrees onto which vegetation will be placed. Measured in world space along the Y axis. |
| `Types` | The types of vegetation for this object. Both VerdantType and VerdantGroup assets can be added here. Your scene can contain a maximum of 31 unique types or groups at any given time. |
| `Ignore Height` | (Mask only) If set, this mask will influence all objects both above and below it regardless of its own placement.  |
| `Type Only` | (Mask only) Makes it so this mask will not influence vegetation scale regardless of the scale value. The mask texture is only used as a mask, meaning it won't affect scale but will still determine wherere vegetation is placed. Useful when placing a type onto a surface in a particular shape without impacting the scale of existing vegetation.    |


### Vegetation Map

|:---------------|:--------------------------|
| `UV Mode` | Determines the space of the mask texture UV coordinates. It can be set to Mesh UV, Local XZ or World XZ. Mesh UV is the regular UVs of your mesh. Local XZ lies on the local XZ axises of the object, meaning they will rotate with the object. World XZ lies on the global XZ axises but is normalized around the center of the object, making it similar to local XZ while ignoring object rotation. |
| `UV Translation` | Move the mask texture in X and Y. |
| `UV Scale` | Scale the mask texture up or down in X and Y. |
| `Scale Mask` | A greyscale texture that controls the scale and placement of vegetation. White is mapped to Min Scale and black is mapped to Max Scale. |
| `Min Scale` | The lowest value of the Scale Mask. Interacts with the overall VerdantObject Scale by multiplication. For example, if Min Scale and Max Scale are 0.5 and 1.0 while Scale is 6.0, the actual scale will range between 3.0 and 6.0 |
| `Max Scale` | The highest value of the Scale Mask. Interacts with the overall VerdantObject Scale by multiplication. For example, if Min Scale and Max Scale are 0.5 and 1.0 while Scale is 6.0, the actual scale will range between 3.0 and 6.0 |

#### Paint Map

|:---------------|:--------------------------|
| `Resolution` | Changes the resolution of the painted texture mask. The contents of your current mask texture will be up or downscaled as needed. |

## Public Methods

|:---------------|:--------------------------|
| ```c#
void AddType(VerdantInstantiable type)
``` | Adds a type into the type list and refreshes the scene. |
| `void RemoveType(VerdantInstantiable type)` | Removes a type from the type list and refreshes the scene. |
| `void RemoveTypeAt(int index)` | Removes the type at the specified index in the type list and refreshes the scene. |
| `void ClearTypes(int index)` | Removes all types from the object and refreshes the scene. |
| `int FindType(VerdantInstantiable type)` | Find the index of the given type in the type list. Returns -1 if it isn't found. |
| `VerdantInstantiable GetType(int index)` | Returns the type at the given index in the type list or throws if the index is out of range. |
| `void SetDirty()` | Marks this object as dirty and makes Verdant refresh the scene at the end of the frame. Needs to be called each time a parameter is changed to apply it. |


