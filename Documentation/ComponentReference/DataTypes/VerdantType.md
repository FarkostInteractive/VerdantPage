---
layout: default
title: "VerdantType"
parent: "Data Types"
grand_parent: "Component Reference"
---

# VerdantType

VerdantTypes contain descriptions of different types of vegetation. You can think of them as a combination of a material and a prefab, controlling both the appearance of the vegetation type and much of its behaviour. At runtime, Verdant uses the description in the VerdantType to dynamically place as many instances as you need across the map. 

At minimum a VerdantType must specify its mesh. From there you can add textures, different LODs, and much more. The parameters and the sections they fall into are explained in detail below. Drawn types can also be influenced at runtime by components in the world, see [affectors](../Affectors/index.html).

A VerdantType becomes active when a [VerdantObject](../VerdantObject) or [VerdantTerrain](../VerdantTerrain) using it comes into the range of a [VerdantCamera](../VerdantCamera). It will then add them into its roster and draw them onto suitable surfaces in the scene. There can be a total of 31 active types per camera at any given time, though it's possible to bypass this limitation by using [VerdantGroups](VerdantGroup.html). The total amount of types in the scene can be much higher as long as only 31 are active at once.

## Performance
VerdantType parameters can have a big impact on how your game performs. If one type dominates your scene it's very likely that setting it up the right way could shave several milliseconds off your frame time. The [Performance guide](../../UserGuide/Performance.html) contains a thorough explanation of types and how they interact with other systems, as well as guidance on how to optimize them for your specific needs. 

## Parameters

|:---------------|:--------------------------|
| `Density` | The number of instances per square meter. This is the base value which both the Falloff and the Coverage Modifier on the VerdantCamera can reduce.   |

### Falloff

The falloff diagram controls how the density changes as the distance from the camera increases. Falloff happens in ten "steps", each of which is the length of 1/10th of the Render Distance. The diagram is read from left to right where left is closest to the camera and right is furthest. It's almost always possible to reduce the density dramatically without it being noticable on the second or third step if the camera is close to the ground.

|:---------------|:--------------------------|
| `Mode` | Lets you select one of three preset falloff modes or Custom, which can be adjusted manually. Which mode to use depends on how close the camera is to the ground. In most cases Sharp or Exponential will be best. Linear makes the amount of vegetation much higher but can work well for aerial views with a smaller render distance. In general, always try to get away with lowering the density as close to the camera as possible.  |

### LOD Parameters

LOD Parameters are parameters that can be different for each LOD. LODs are edited using the segments under the Falloff graph. The plus button lets you add new LODs and right clicking on a segment lets you remove it. The width of the bar represents how many falloff steps the LOD stretches over and can be changed by dragging the handles. You can click on a bar to select it and show its parameters. 

The parameters are optional on each LOD but the first, which is marked by the checkboxes to the right of each field. If the checkbox is not set the parameter will inherit the value of the prior LOD. This way you only need to set parameters that change for later LODs. Everything else will be brought over automatically.

The Mesh, Shadow Mesh, Override Shader and Override Material parameters do not have a parameter checkbox but are still optional. The previous LOD value will be used if they are left unset. If the shader and material are left empty on the first LOD Verdant will fall back to the default shader. If no Shadow Mesh is set the main mesh will be used for shadows.

|:---------------|:--------------------------|
| `LOD Fade` | Controls how much this LOD zone should fade into the next one. The overlap between the two will cause the amount of instances rendered in this LOD to increase, as it will force it to bleed into the next LOD zone. This incurs a slight performance cost, but can be very worth it if it enables you to use stricter LODs. |
| `Mesh` | The mesh to use. This is the only parameter on a type that *must* be set or the type won't be able to render. |
| `Shadow Mesh` | Allows you to specify a mesh to use for the shadow pass. Rendering shadows is expensive, so providing a simpler mesh here can improve performance significantly. If left unset, the main mesh will be used. Shadows will only render when enabled by the Cast Shadows parameter. |
| `Override Shader` | Sets the shader to use with the VerdantType. By default this is the Verdant Standard Shader. Regular shaders won't work here, they need to be adapted to include some Verdant code for placement and parameter application. [Writing Custom Shaders](../../AdvancedGuide/WritingCustomShaders) explains how.  |
| `Override Material` | Like the Override Shader parameter, but also includes the parameters on the material. The material shader will be used as the Override Shader, and all the material parameters will be applied along with the VerdantType parameters. Verdant will create a copy of the material on type instantiation, so changes made to the material after the type is added to the scene won't be applied. This is the easiest way to include custom parameters with your custom shaders. |
| `Shading Level` | Allows you to enable and disable certain features in the shader. Verdant always tries to avoid unnecessary work, but this setting guarantees that features you don't need won't be used. Setting it as low as possible can give you significantly better performance. Disabled parameters will be greyed out. |
| `Allow Alpha Clip` | Allows you to enable and disable alpha testing. This should always be disabled unless you absolutely need it, as alpha has impacts VerdantType performance across the board. Disabled parameters will be greyed out. |

#### Surface Properties

Surface Properties are LOD properties that influence how the surface of the type looks. Most of these are similar to the parameters on the Unity standard shader, and you can generally think of them as material properties.

|:---------------|:--------------------------|
| `Color` | The diffuse color of the type. Gets multiplied into the diffuse texture. |
| `Texture` | The main diffuse texture of the type. |
| `Opacity Dithering` | If alpha clipping is enabled, this parameter controls how much the values between 0 and 1 should dither the instance. 0 disables dithering and 1 enables it fully. |
| `Translucency Map` | A greyscale texture that controls the amount of translucency for each pixel. Translucency acts slightly different in Field Volumetric mode, where increasing Field Radiance will make it act more as the translucency of the field as a whole. This manifests as a specular or rim light - like effect on translucent parts of the VerdantType.  |
| `Translucency` | Controls the amount of translucency. If a Translucency texture is set this parameter multiplies it. |
| `Diffusion` | Controls how much the translucency light bounces within the VerdantType. |
| `Normal Map` | The normal map of the type. Disabled in the Basic Shading Level.  |
| `Normal Strength` | Controls the strength of the normal map if one is set. |
| `Metallic Map` | Equivalent to the Metallic texture in the Unity standard shader. Disabled in all Shading Levels except Full Surface Detail. |
| `Smoothness` | Equivalent to the smoothness slider on the Unity standard shader. |
| `Metallic` | Equivalent to the metallic slider on the Unity standard shader. |
| `Opacity Map` | A separate greyscale texture used to control opacity. Disabled in all Shading Levels except Full Surface Detail. |
| `Shadow Opacity Map` | Can be used to override the Opacity Map in the shadow pass. Useful for rendering shadows as simpler meshes like billboards. |
| `Occlusion Map` | Equivalent to the occlusion texture on the Unity standard shader. Disabled in all Shading Levels except Full Surface Detail. |
| `Occlusion` | Controls the strength of the occlusion map if one is set. |

#### Instance Properties

LOD properties that influence the placement, animation and rendering of whole instances rather than just their surfaces.

|:---------------|:--------------------------|
| `Billboarding` | Controls how strongly this type should rotate around the Y axis to face the camera. 0 disables billboarding and 1 sets the type to always face the camera. |
| `Scale` | Sets how large this type should be in the world. This is a base value which can be modified by a number of other scales, like on VerdantObject and scale affectors. |
| `Deflection Angle` | Controls how far in degrees this type will bend under the influence of a deflection affector.  |
| `Color Field Influence` | Controls how strongly the color field influences the color of this type. |
| `Scale Field Influence` | Controls how strongly the scale field influences the scale of this type. |
| `Stiffness` | Used to calculate how this type is influenced by wind volumes. A higher number makes the type harder to bend and less susceptible to wind. |
| `Light Mode` | Determines how this type will be lit. Translucent is a modified version of the Unity Standard Shader with added support for double sided geometry and translucency. Field Volumetric does everything that Translucent does, but also takes the surrounding vegetation into account to approximate shadows and bounce lighting. You will generally want to use Field Volumetric for types that grow in dense fields and Translucent for types that are more sparse and independent.
| `Fade Mode` | Determines how new instances fade in as they are approached. Scale scales them up from zero. Dither is available when alpha is enabled and will use a dither mask to fade in instances pixel by pixel. |
| `Receive Shadows` | Sets if this type should receive shadows. In deferred rendering all objects always receive shadows, so this parameter is only used in forward rendering. |
| `Cast Shadows` | Controls if and how the type should cast shadows. Shadow casting is one of the most expensive features in Verdant because it causes all vegetation to be redrawn at least once for each shadowing light. Try to only use it on your first LOD. The Field Volumetric light mode can approximate low detail shadow casting at a much lower performance cost. |

#### Field Lighting Properties

These settings are used in the Field Volumetric light mode and control how instances of this type are influenced by their surroundings. There are two main factors: Light being blocked by neighbours (shadowing) and light being reflected by neighbours (radiance). 

You can imagine that each field volumetric instance is surrounded by a cylinder that represents its neighbours. The cylinder can both block rays of lights to cast shadows on the instance and reflect bounced light back onto it. The upper edge of the cylinder varies in height depending on the size and height of the neighbours.

The parameters below control the properties of the cylinder wall and the light reflected off it.

|:---------------|:--------------------------|
| `Field Radiance` | The proportion of the light hitting this instance that is bounced from its neighbours. Higher radiance makes light softer and more consistent across a body of vegetation, but blows out some of the per pixel detail from the surface property maps. |
| `Field Shadow Height` | A multiplier on the height of the shadowing wall. At 1 it will be as high as the type mesh. |
| `Field Shadow Softness` | Controls how hard the edge of the field shadow is. Generally, the less regular the shape of the vegetation type is the softer its shadows should be. Hard shadows have an almost cartoony look while soft ones are appropriate for more realistic lighting conditions. |
| `Field Shadow Strength` | Controls how dark the field shadow is. Roughly equivalent to the shadow strength parameter on regular Unity lights. |
| `Field Shadow Distance` | Controls how far away the shadowing wall is. If the wall is very close to the instance the instance will almost always be in shadow and light will only be let in from the very top. If the wall is further away, the angle at which light is let in increases. The higher this parameter is the less prominent the field shadows are, and lighting will vary more as the light direction changes. |

### Type Parameters

Finally, these parameters are consistent across all LODs and can only be set once per VerdantType.

|:---------------|:--------------------------|
| `Layer` | The layer of this type. If the [VerdantCamera](../VerdantCamera) specifies an override layer that will be used instead. |
| `Placement Randomness` | By default Verdant vegetation is placed in a tightly packed pattern optimized for coverage. This parameter lets you add a random offset to the base position, which can make sparse vegetation placement look more natural. |