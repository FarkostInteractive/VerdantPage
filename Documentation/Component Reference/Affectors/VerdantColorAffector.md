---
layout: default
title: "VerdantColorAffector"
nav_order: "1"
parent: "Affectors"
grand_parent: "Component Reference"
---

# VerdantColorAffector
Used to interactively color vegetation.

For more information about affectors in general, see the [Affectors page](index.html). 

## Parameters

|:---------------|:--------------------------|
| `Ignore Height` | If set, the affector will always influence the vegetation regardless of how close it is to the ground.  |
| `Ground Distance` | Determines how close to the ground the affector needs to be to influence vegetation. This applies both when the affector is above and below ground. |
| `Blend Mode` | Sets how the affector colors should blend with the existing colors in the field. Set will simply overwrite what is there, whereas Add and Multiply will use the respective operation to blend. |
| `Color` | The color which the affector will apply to the vegetation. Will be overridden by the texture if one is used. |

### Texturing

|:---------------|:--------------------------|
| `Use Texture` | Should this affector use a texture to determine its color? |
| `Texture` | The texture to use. |
| `UV Mode` | Determines the space of the texture UV coordinates. It can be set to Mesh UV, Local XZ or World XZ. Mesh UV is the regular UVs of your mesh. Local XZ is the object space XZ axises. World XZ will make the texture repeat along the XZ axises in world space. |
| `UV Translation` | Moves the color texture in X and Y. |
| `UV Scale` | Scales the color texture up or down in X and Y. |
| `Remap Texture` | If set, the texture will be treated as a greyscale texture where black maps to Texture Min Color and white maps to Texture Max Color.  |
| `Texture Min Color` | The minimum color used for a remapped texture. |
| `Texture Max Color` | The maximum color used for a remapped texture. |

### Clamping

|:---------------|:--------------------------|
| `Clamp` | Should the color under this affector be clamped? |
| `Min Color` | The minimum clamp color |
| `Max Color` | The maximum clamp color |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Marks this affector as dirty so it will be redrawn in the color field next frame. Must be called for changed parameters to take effect. |


