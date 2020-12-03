---
layout: default
title: "VerdantScaleAffector"
nav_order: "2"
parent: "Affectors"
grand_parent: "Component Reference"
---

# VerdantScaleAffector
Used to scale vegetation interactively. 

For more information about affectors in general, see the [Affectors page](index.html). 

## Parameters

|:---------------|:--------------------------|
| `Ignore Height` | If set, the affector will always influence the vegetation regardless of how close it is to the ground.  |
| `Ground Distance` | Determines how close to the ground the affector needs to be to influence vegetation. This applies both when the affector is above and below ground. |
| `Blend Mode` | Sets how the affector should influence the existing scale field. Set will simply overwrite what is there, whereas Add and Multiply perform their respective operation. |
| `Scale` | The scale which the affector will apply to the vegetation. Will be overridden by the texture if one is used. |

### Texturing

|:---------------|:--------------------------|
| `Use Texture` | Should this affector use a texture to determine its scale? |
| `Texture` | A greyscale texture to use to determine scale. |
| `UV Mode` | Determines the space of the texture UV coordinates. It can be set to Mesh UV, Local XZ or World XZ. Mesh UV is the regular UVs of your mesh. Local XZ is the object space XZ axises. World XZ will make the texture repeat along the XZ axises in world space. |
| `UV Translation` | Moves the scale texture in X and Y. |
| `UV Scale` | Scales the scale texture up or down in X and Y. |
| `Remap Texture` | If set, the texture will be remapped from scales 0 to 1 for black and white to the Texture Min Value and Texture Max Value.  |
| `Texture Min Value` | The minimum color used for a remapped texture. |
| `Texture Max Value` | The maximum color used for a remapped texture. |

### Clamping

|:---------------|:--------------------------|
| `Clamp` | Should the scale under this affector be clamped? |
| `Min Value` | The minimum clamp scale |
| `Max Value` | The maximum clamp scale |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Marks this affector as dirty so it will be redrawn in the scale field next frame. Must be called for changed parameters to take effect. |


