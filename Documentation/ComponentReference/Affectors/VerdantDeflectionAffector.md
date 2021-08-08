---
layout: default
title: "VerdantDeflectionAffector"
nav_order: "0"
parent: "Affectors"
grand_parent: "Component Reference"
---

# VerdantDeflectionAffector
An affector that bends vegetation around objects interactively. Use it to make grass move around the feet of a character, flatten it under a box or add gusts of wind from the exhausts of a spaceship. To use it you will need a [VerdantDeflectionField](../Fields/VerdantDeflectionField.html) component on your VerdantCameras.  

Deflection has two different force modes: Push and Press. Push is a force that passes through vegetation, like blowing air or a person brushing against grass. While a push can have different strengths and be strong enough to keep vegetation down it can never completely flatten it. That is what Press is for. Press will instantly flatten any vegetation under it and keep it down for a set duration, after which it will release and come bouncing back.

Deflection affectors differ from other affectors in that they are always drawn at a regular interval. This is necessary because the field is always running its simulation. If a color affector draws a red splotch it will be there until something else paints over it, so it only needs to be drawn once. But the deflection field is always trying to restore itself so the affectors must be applied continuously, just as objects in the real world exert forces even when static. You must still call SetDirty when changing parameters, but changes to the mesh or texture will be picked up automatically.

For more information about affectors in general, see the [Affectors page](index.html). 

## Straight Write
When using a texture for deflection there is one available option that needs a bit of additional explaining. Straight Write will write the texture exactly as it is into the field, skipping the interpretation steps performed by the other modes. This is a bit of an experimental feature and is intended as a way for the user to add more complex wind systems. One such use case would be writing a full GPU fluid simulator and rendering it out into a texture as deflection data understandable to Verdant. By adding the resulting render texture onto a deflection affector with its shape set to Map its data gets passed into Verdant and overrides the regular simulation. It can also be used for simpler cases, like displaying baked wind animations or procedurally generating patterns with a shader.

Deflection is modeled as a spring and represented as two 2D vectors in the XZ plane: The position and the velocity. Both use half precision floats, meaning that unlike a regular texture they can be negative numbers and/or higher than 1. Keep this in mind when creating your own render texture.

Position here is how extended the spring is in each direction and gets unpacked into a direction and a rotation value when read by vegetation. It is normalized from 0 to 1, but the length is also used as the press timer. If the press time is 1 the length gets set to 2 and then counted down and released when smaller than 1. Position is stored in the XY components. 

Velocity is a more straightforward value and represents the velocity of the spring as it was in the previous simulation step. The velocity can be however high it needs to be, but when applied will never push the position beyond length 1. When a push force affects the field this is the vector it writes into. It is stored in the ZW components.

To take complete control of the simulation, set XY to the direction you want and ZW to zero. If applicable setting the length of XY to higher than 1 will ensure Verdant does nothing for the pixels drawn to, though setting the velocity should suffice in most cases.

## Parameters

|:---------------|:--------------------------|
| `Ignore Height` | If set, the affector will always influence the vegetation regardless of how close it is to the ground.  |
| `Ground Distance` | Determines how close to the ground the affector needs to be to influence vegetation. This applies both when the affector is above and below ground. |
| `Force Mode` | Sets if vegetation should be pushed or pressed down. A push is a force that can be gentle or strong, but always stops the moment the affector is disabled or moves away. A press is the equivalent of a heavy object resting on the field and will completely flatten vegetation below it. The amount of time it takes to raise pressed vegetation can be specified with the Press Duration parameter. |
| `Force` | (Force mode Push only) The amount of force to use. Its effectiveness will depend on the DeflectionField simulation settings. |
| `Press Duration` | (Force mode Press only) The time in seconds for which the vegetation should stay pressed down. |
| `Direction` | Sets whether vegetation should deflect along the objects normals or its direction of motion. Will be overridden by the texture if one is used. |
| `Speed Influence` | (Force mode push only) The degree to which the speed of this object will influence the force. At 1 an immobile object will assert no force. At 0 the object will assert the specified force regardless of its speed. |


### Texturing

|:---------------|:--------------------------|
| `Use Texture` | Should this affector use a texture to determine the direction vegetation should deflect in? |
| `Texture Type` | Decides how to interpret the data in the texture. Color Texture will use the R and G channels of a texture as the direction. Normal map will unpack the texture as it would a normal map and use the directions there. Straight Write will write the texture exactly as it is into the field. This can be used for special effects, see the section above for details on how. |
| `Texture` | The texture to use for deflection. It will be interpreted using the Texture Type parameter. |
| `UV Mode` | Determines the space of the texture UV coordinates. It can be set to Mesh UV, Local XZ or World XZ. Mesh UV is the regular UVs of your mesh. Local XZ is the object space XZ axises. World XZ will make the texture repeat along the XZ axises in world space. |
| `UV Translation` | Moves the deflection texture in UV space X and Y. |
| `UV Scale` | Scales the deflection texture up or down in UV space X and Y. |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Marks this affector as dirty so it will be redrawn in the deflection field next frame. Must be called for changed parameters to take effect. |


