---
layout: default
title: "VerdantDeflectionAffector"
nav_order: "0"
parent: "Affectors"
grand_parent: "Component Reference"
---

# VerdantDeflectionAffector
Used to bend vegetation around objects interactively. 

For more information about affectors in general, see the [Affectors page](index.html). 

## Parameters

|:---------------|:--------------------------|
| `Ignore Height` | If set, the affector will always influence the vegetation regardless of how close it is to the ground.  |
| `Ground Distance` | Determines how close to the ground the affector needs to be to influence vegetation. This applies both when the affector is above and below ground. |

### Texturing

|:---------------|:--------------------------|
| `Use Texture` | Should this affector use a texture to determine the direction vegetation should deflect in? |
| `Texture Type` | Decides how to interpret the data in the texture. Color Texture will use the R and G channels of a texture as the direction. Normal map will unpack the normals and use them. Straight Write will write the texture exactly as it is into the field. This can be used for special effects. Check the [VerdantDeflectionField documentation](../Fields/VerdantDeflectionField.html) for details on how to do this. |
| `Texture` | The texture to use for deflection. It will interpreted using the Texture Type parameter. |
| `UV Mode` | Determines the space of the texture UV coordinates. It can be set to Mesh UV, Local XZ or World XZ. Mesh UV is the regular UVs of your mesh. Local XZ is the object space XZ axises. World XZ will make the texture repeat along the XZ axises in world space. |
| `UV Translation` | Moves the deflection texture in X and Y. |
| `UV Scale` | Scales the deflection texture up or down in X and Y. |

|:---------------|:--------------------------|
| `Direction` | Sets whether vegetation should deflect along the objects normals or its direction of motion. Will be overridden by the texture if one is used. |
| `Force Mode` | Sets if vegetation should be pushed or pressed down. A push is a force that can be gentle or strong, but always stops the moment the affector is disabled or moves away. A press is the equivalent of a heavy object resting on the field and will completely flatten vegetation below it. The amount of time it takes to raise pressed vegetation can be specified with the Press Duration parameter. |
| `Force` | (Force mode push only) The amount of force to use. Its effectiveness will depend on the DeflectionField simulation settings. |
| `Press Duration` | (Force mode press only) The time in seconds for which the vegetation should stay pressed down. |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Marks this affector as dirty so it will be redrawn in the scale field next frame. Must be called for changed parameters to take effect. |


