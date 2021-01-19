---
layout: default
title: "VerdantType"
parent: "Data Types"
grand_parent: "Component Reference"
---

# VerdantType

VerdantTypes are templates for vegetation that can be placed into the world. You can think of them as a combination of a material and a prefab, as they contain both parameters for visuals and for how instances should be placed, animated, interacted with, and so on. At runtime VerdantCamera will use the data here to draw hundreds of thousands of instances procedurally.

At minimum a VerdantType must specify its mesh. From there you can add textures, different LODs, and much more. The parameters and the sections they fall into are explained in detail below. Drawn types can also be influenced at runtime by components in the world, see [affectors](../Affectors/index.html).

A VerdantType becomes active when a VerdantObject or VerdantTerrain using it comes into the range of a VerdantCamera. It will then add them into its roster and draw them onto suitable surfaces in the scene. There can be a total of 31 active types per camera at any given time, though it's possible to bypass this limitation by using [VerdantGroup](VerdantGroup.html). The total amount of types in the scene can be much higher as long as only 31 are active at once.

## Performance
VerdantType parameters can have a big impact on how your game performs. If one type dominates your scene it's very likely that setting it up the right way could shave several milliseconds off your frame time. The [Performance guide](../../UserGuide/Performance.html) contains a thorough explanation of types and how they interact with other systems, as well as guidance on how to optimize them for your specific needs. 

One point in particular is worth reiterating here: Using Allow Alpha and Shading Level. You should always set them as low as you can. Disabling Allow Alpha especially can almost double your frame rate under some circumstances, as it allows the shader to perform early Z-testing and do much less work. You should also be careful to not use shadows unless you really need them.

## Parameters

|:---------------|:--------------------------|
| `Density` | The number of instances per square meter. This is the basis which both the Falloff and the Coverage Modifier on the [VerdantCamera](../VerdantCamera.html) can reduce.   |

### Falloff

The falloff diagram controls how the density changes as the distance from the camera increases. Falloff happens in ten "steps" each of which is the length of 1/10th of the Render Distance. It is read from left to right where left is closest to the camera and right is furthest. It's almost always possible to reduce the density dramatically on the second or third step if the camera is close to the ground.

|:---------------|:--------------------------|
| `Mode` | Lets you select one of three preset falloff modes or Custom, which can be adjusted manually. Which mode to use depends on how close the camera is to the ground. In most cases Sharp or Exponential will be best. Linear makes the amount of vegetation much higher but can work well for aerial views with a smaller render distance. In general, always try to get the density down as low as possible as close to the camera as possible.  |

### LOD Parameters

LOD Parameters are parameters that can be different for each LOD. LODs are edited using the segments under the Falloff graph. The plus button lets you add new LODs and right clicking on a segment lets you remove it. The width of the bar represents how many falloff steps the LOD stretches over and can be changed by dragging the handles. You can click on a bar to select it and show its parameters. 

The parameters are optional on each LOD but the first, which is marked by the checkbox to their right. If the checkbox is not set the parameter will inherit the value of the prior LOD. This way you only need to set parameters that change for later LODs, which is often limited to parameters like the mesh, shadow mode and billboarding. Everything else will be brought over so you only need to set them once.

The Mesh, Override Shader and Override Material do not have a parameter checkbox but are still optional. The previous LOD value will be used if they are left unset. 

|:---------------|:--------------------------|
| `LOD Fade` | Controls how much this LOD zone should fade into the next one. They will overlap slightly and render more instances than strictly necessary, but this enables them to swap them out per instance rather than in entire chunks and makes the transition between LODs much smoother. If it lets you use stricter LODs it can be very worth it to take the slight performance cost of the fade. |
| `Mesh` | The mesh to use. This is the only parameter on a type that *must* be set or the type won't be able to render. |
| `Override Shader` | Lets you set a different shader than the Verdant Standard Shader to use. Regular shaders won't work here, they need to include some Verdant code for placement and applying parameters. [Writing Custom Shaders]() will take you through this process.  |
| `Override Material` | Uses the materials' shader as an override and applies all the parameters set on the material. Verdant will create new instances of the material, so changes made after the type is added to the scene won't be applied. Using an override material is the easiest way to add custom parameters to Verdant. |
| `Shading Level` | Allows you to enable and disable certain features at the shader level. Verdant always tries to avoid unnecessary work, but this setting guarantees that features you don't need won't be used. Setting it as low as possible can give you significantly better performance. Disabled parameters will be greyed out. |
| `Allow Alpha` | Allows you to enable and disable alpha testing. This should always be disabled unless you absolutely need alpha as it has a bigger impact on performance than any other type parameter. Disabled parameters will be greyed out. |

#### Surface Properties

LOD properties that influence how the surface of the type looks. Most of these are similar to the parameters on the Unity standard shader. You can think of them as material properties.

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

LOD properties that influence the placement, animation and rendering of instances. These are more like parameters to the vertex shader if the Surface Properties are pixel shader paramters.

|:---------------|:--------------------------|
| `Billboarding` | Controls how strongly this type should rotate around the Y axis to face the camera. 0 disables billboarding and 1 sets it to always face the camera. |
| `Scale` | Sets how large this type should be in the world. This is a basis which can be modified by a number of other scales, like VerdantObject and scale affectors. |
| `Deflection Angle` | Controls how far in degrees this type will bend under the influence of a deflection affector.  |
| `Color Field Influence` | Controls how strongly the color field influences the color of this type. |
| `Scale Field Influence` | Controls how strongly the scale field influences the scale of this type. |
| `Stiffness` | Used to calculate how this type is influenced by wind volumes. A higher number makes the type harder to bend and less susceptible to wind. |
| `Light Mode` | Determines how this type will be lit. Normal Up sets all of the normals in the mesh to point upwards, which has the effect of lighting the type as if it were part of the surface it's placed on. This mode is almost always best for dense vegetation like grass as you usually want its lighting to match the ground. Translucent is a more complex model that keeps the mesh normals but lights it both from the front and the back. The translucency parameter and texture control how much light passes through. This mode is ideal for large leafy plants. |
| `Ground Normals` | Only available when the light mode is set to normal up. When set to zero the normal of the instance will point along the Y axis. When set to 1 it will point along the normal of the surface it is placed on. |
| `Fade Mode` | Determines how new instances fade in as they are approached. Scale simply scales them up from zero. Dither is available when alpha is enabled and will use a dither mask to fade in instances pixel by pixel. |
| `Pivot Mode` | Sets what the instance considers to be its pivot point. For most types you want this set to Origin. For meshes that resemble a flat and wide patch of vegetation Projected will try to estimate a pivot for each vertex and use that. This has the effect of bending the mesh along the ground surface and can make wind and affector influences look much more natural. How right the estimation is depends on the mesh. Complicated meshes can easily become very broken in Projected. |
| `Receive Shadows` | Sets if this type should receive shadows. In deferred rendering all objects always receive shadows, so this parameter is only relevant in forward rendering. |
| `Cast Shadows` | Controls if and how the type should cast shadows. Shadow casting is one of the most expensive features in Verdant because it causes all vegetation to be redrawn at least once for each shadowing light. If you can, try to only use it on your first LOD. |

### Type Parameters

Finally, these parameters are consistent across all LODs and can only be set once per VerdantType.

|:---------------|:--------------------------|
| `Layer` | The layer of this type. If the [VerdantCamera](../VerdantCamera.html) specifies an override layer that will be used instead. |
| `Placement Randomness` | By default Verdant vegetation is placed in a tightly packed pattern optimized for coverage. This parameter lets you add a random offset to the base position, which can make sparse vegetation placement look more natural. |
| `Twin Object` | Sets the Twin Object of the type, which is a prefab that will replace GPU instances as the camera approaches them. The prefab must have a component inheriting VerdantGameObjectTwin. [The Twin Object guide](../../AdvancedGuide/UsingTwinObjects.html) takes you through the process of setting one up. |