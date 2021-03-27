---
layout: default
title: "VerdantObject"
parent: "Component Reference"
nav_order: "1"
---

# VerdantObject
This is the main component for putting Verdant vegetation into a scene. It defines zones where vegetation should be placed, which is the main mode of interaction in Verdant. 

Types are not placed one by one, they are automatically added en masse at runtime to areas defined in the editor. When an object has a VerdantObject component it will be recognized as a surface on which vegetation can grow or as a shape that influences where vegetation should grow. To facilitate this it has two modes: Surface and Mask.

Surface objects place vegetation onto themselves. By default any type added to them grows to cover the top of the mesh at the density set in the type. This serves as a base for more complex patterns, which can be made using masks.

Mask objects influence the vegetation of the Surface objects below them. Any types added to the mask will be placed onto the surfaces below and its Scale parameter will be multiplied onto the surface. It is the shape of the mesh itself that becomes the mask, meaning Verdant can be used along with any system you might be using to make prototype meshes.

Both modes can also have a mask texture applied to them to further control placement. The mask can either be set by the parameter Scale Mask or painted directly using the controls under Paint Mask.

This component is meant to be used with Mesh Renderer and/or Verdant Shape Descriptor. If you want to use Verdant with terrains you have to use the similar VerdantTerrain component instead. There is currently no support for skinned mesh renderers.

## Painting a Mask Texture
You can find the tools for mask texture painting either in the VerdantObject inspector under Paint Mask or in the Scene View Tools shelf (can be enabled or disabled with the tools icon in the upper right corner). When you first use them you will be asked to initialize the mask texture. Doing so will create a new data structure that is serialized along with the component. See the page [Painting Mask Textures](../AdvancedGuide/PaintingMaskTextures.html) for details on how to use the paint tools.

## Parameters

|:---------------|:--------------------------|
| `Mode` | Determines whether vegetation should be placed onto this object (Surface) or onto the VerdantObjects below it (Mask). |
| `Scale` | Sets the scale of vegetation for this object. Interacts with other scale parameters (eg. on the VerdantType) by multiplication. |
| `Max Slope` | The steepest slope in degrees onto which vegetation will be placed, measured in world space along the Y axis. |
| `Ignore Height` | (Mask only) If set, this mask will influence all objects both above and below it regardless of its own placement.  |
| `Type Only` | (Mask only) Makes it so this mask will not influence vegetation scale regardless of the scale value. The mask texture is only used as a mask, meaning it won't affect scale but will still determine where vegetation is placed. This allows for placing a type onto a surface in a particular shape without impacting the scale of existing vegetation. |
| `Types` | The types of vegetation for this object. Both VerdantType and VerdantGroup assets can be added here. The rendered scene can contain a maximum of 31 unique types or groups at any given time. |


### Masking Texture

|:---------------|:--------------------------|
| `UV Mode` | Determines the space of the mask texture UV coordinates. It can be set to Mesh UV, Local XZ or World XZ. Mesh UV is the regular UVs of your mesh. Local XZ lies on the local XZ axises of the object, meaning they will rotate with the object. World XZ lies on the global XZ axises but is normalized around the center of the object, making it similar to local XZ while ignoring object rotation. |
| `UV Translation` | Moves the mask texture in UV space X and Y. |
| `UV Scale` | Scales the mask texture up or down in UV space X and Y. |
| `Scale Mask` | A greyscale masking texture that controls the scale and placement of vegetation. Black is mapped to Min Scale and white is mapped to Max Scale. |
| `Min Scale` | The lowest value of the Scale Mask. Interacts with the overall VerdantObject Scale by multiplication. For example, if Min Scale and Max Scale are 0.5 and 1.0 while Scale is 6.0, the actual scale will range between 3.0 and 6.0 |
| `Max Scale` | The highest value of the Scale Mask. Interacts with the overall VerdantObject Scale by multiplication. For example, if Min Scale and Max Scale are 0.5 and 1.0 while Scale is 6.0, the actual scale will range between 3.0 and 6.0 |

#### Painted Mask

|:---------------|:--------------------------|
| `Resolution` | Changes the resolution of the painted texture mask. The contents of your current mask texture will be up or downscaled as needed. |

## Public Methods

|:---------------|:--------------------------|
| `void AddType(VerdantInstantiable type)` | Adds a type into the type list and refreshes the scene. |
| `void RemoveType(VerdantInstantiable type)` | Removes a type from the type list and refreshes the scene. |
| `void RemoveTypeAt(int index)` | Removes the type at the specified index in the type list and refreshes the scene. |
| `void ClearTypes(int index)` | Removes all types from the object and refreshes the scene. |
| `int FindType(VerdantInstantiable type)` | Find the index of the given type in the type list. Returns -1 if it isn't found. |
| `VerdantInstantiable GetType(int index)` | Returns the type at the given index in the type list or throws if the index is out of range. |
| `void SetDirty()` | Marks this object as dirty and makes Verdant refresh the scene at the end of the frame. Needs to be called each time a parameter is changed to apply it. |


