---
layout: default
title: "VerdantType"
parent: "Data Types"
grand_parent: "Component Reference"
---

# VerdantType

Represents a single type of Verdant vegetation that can be placed into the world. 

## Parameters

|:---------------|:--------------------------|
| `Density` | The number of instances per square meter. This is the basis which both the Falloff and the Coverage Modifier on the [VerdantCamera](../VerdantCamera.html) can reduce.   |

### Falloff

|:---------------|:--------------------------|
| `Mode` | Lets you select one of three preset falloff modes or Custom, which can be adjusted manually. Which mode to use depends on how close the camera is to the ground. In most cases Sharp or Exponential will be best. Linear makes the amount of vegetation much higher but can work well for aerial views with a smaller render distance. In general, always try to get the density down as low as possible as quickly as possible.  |

### LOD Parameters

|:---------------|:--------------------------|
| `LOD Fade` | Controls how much this LOD zone should fade into the next one. They will overlap slightly and render more instances than strictly necessary, but this enables them to swap them out per instance rather than in entire chunks and makes the transition between LODs much smoother. If it lets you use stricter LODs it can be very worth it to take the slight performance cost of the fade. |
| `Mesh` | The mesh to use. This is the only parameter on a type that *must* be set or the type won't be able to render. |
| `Override Shader` | Lets you set a different shader than the Verdant Standard Shader to use. Regular shaders won't work here, they need to include some Verdant code for placement and applying parameters. [Writing Custom Shaders]() will take you through this process.  |
| `Override Material` | Uses the materials' shader as an override and applies all the parameters set on the material. Verdant will create new instances of the material, so changes made after the type is added to the scene won't be applied. Using an override material is the easiest way to add custom parameters to Verdant. |
| `Shading Level` | Allows you to enable and disable certain features at the shader level. Verdant always tries to avoid unnecessary work, but this setting guarantees that features you don't need won't be used. Setting it as low as possible can give you significantly better performance. Disabled parameters will be greyed out. |
| `Allow Alpha` | Allows you to enable and disable alpha testing. This should always be disabled unless you absolutely need alpha as it has a bigger impact on performance than any other type parameter. Disabled parameters will be greyed out. |

#### Surface Properties

|:---------------|:--------------------------|
| `Color` | A color that gets multiplied into the texture |
| `Texture` | The main diffuse texture of the type. |
| `Opacity Dithering` | If alpha clipping is enabled, this parameter controls how much the values between 0 and 1 should dither the instance. 0 disables dithering and 1 enables it fully. |
| `Translucency (Texture)` | A greyscale texture that controls the amount of translucency for each pixel. Translucency is a special Verdant effect which is expressed differently depending on the Light Mode used. In translucent it simply controls how much light gets let through from the backside of a pixel. In Normal Up it becomes an effect analogous to specularity on large bodies of water or sand that makes pixels brighter when the camera is looking towards a light. It can be used to create the effect of light filtering through blades of grass. Disabled in the Basic Shading Level. |
| `Translucency` | Controls the amount of translucency. If a Translucency texture is set this parameter multiplies it. |
| `Diffusion` | Only available in the Normal Up light mode. Controls how focused the specularity-like cone of light is. |
| `Normal Map` | The normal map of the type. Available in both Light Modes. Disabled in the Basic Shading Level.  |
| `Metallic (Texture)` | Equivalent to the Metallic texture in the Unity standard shader. Disabled in all Shading Levels except Full Surface Detail. |
| `Smoothness` | Only available in the Translucent light mode. Equivalent to the smoothness slider on the Unity standard shader. |
| `Metallic` | Equivalent to the metallic slider on the Unity standard shader. |
| `Opacity (Texture)` | A separate greyscale texture used to control opacity. Disabled in all Shading Levels except Full Surface Detail. |
| `Occlusion Map` | Equivalent to the occlusion texture on the Unity standard shader. Disabled in all Shading Levels except Full Surface Detail. For deferred rendering reasons a pixel cannot be both occluded and translucent. If both are active at once occlusion will take prescedence. |

#### Instance Properties

|:---------------|:--------------------------|
| `Billboarding` | Controls how strongly this type should rotate around the Y axis to face the camera. |
| `Scale` | Sets how large this type should be in the world. A basis which can be modified by a number of other scales, like VerdantObject and scale affectors. |
| `Deflection Angle` | Controls how far in degrees this type will bend under the influence of a deflection affector.  |
| `Color Field Influence` | Controls how strongly the color field influences the color of this type. |
| `Scale Field Influence` | Controls how strongly the scale field influences the scale of this type. |
| `Stiffness` | Used to calculate how this type is influenced by wind volumes. A higher number makes the type harder to bend and less susceptible to wind. |
| `Light Mode` | Determines how this type will be lit. Normal Up sets all of the normals in the mesh to point upwards, which has the effect of lighting the type as if it were part of the surface it's placed on. This mode is almost always best for dense vegetation like grass as you usually want its lighting to match the ground. Translucent is a more complex model that keeps the mesh normals but lights it both from the front and the back. The translucency parameter and texture control how much light passes through. This mode is ideal for large leafy plants. |
| `Ground Normals` | Only available when the light mode is set to normal up. When set to zero the normal of the instance will point along the Y axis. When set to 1 it will point along the normal of the surface it is placed on. |
| `Fade Mode` | Determines how new instances fade in as they are approached. Scale simply scales them up from zero. Dither is available when alpha is enabled and will use a dither mask to fade in instances pixel by pixel. |
| `Pivot Mode` | Sets what the instance considers to be its pivot point. For most types you want this set to Origin. For meshes that resemble a flat and wide patch of vegetation Projected will try to estimate a pivot for each vertex and use that. This has the effect of bending the mesh along the ground surface and can make wind and affector influences look much more natural. How right the estimation is depends on the mesh. Complicated meshes can easily become very broken in Projected. |
| `Receive Shadows` | Sets if this type should receive shadows. In deferred rendering all objects always receive shadows, so this parameter is only relevant in forward rendering. |
| `Cast Shadows` | Controls if and how the type should cast shadows. Shadow casting is one of the most expensive features in Verdant because it causes all vegetation to be redrawn at least once for each shadowing light. If you can, try to only use it on your first LDO. |

### Type Parameters

|:---------------|:--------------------------|
| `Layer` | The layer of this type. If the [VerdantCamera](../VerdantCamera.html) specifies an override layer that will be used instead. |
| `Placement Randomness` | By default Verdant vegetation is placed in a tightly packed pattern optimized for coverage. This parameter lets you add a random offset to the base position, which can make sparse vegetation placement look more natural. |
| `Twin Object` | Sets the Twin Object of the type, which is a prefab that will replace GPU instances as the camera approaches them. The prefab must have a component inheriting VerdantGameObjectTwin. [The Twin Object guide](../../UserGuide/UsingTwinObjects.html) takes you through the process of setting one up. |