---
layout: default
title: "VerdantScaleField"
parent: "Fields"
grand_parent: "Component Reference"
---

# VerdantScaleField

The scale field is multiplied into all the other scales that might be acting on your vegetation, like the VerdantObject scale and base VerdantType scale. It is 1 per default if no affector is active and no Base Texture is set.

Objects that want to interact with this field need to have [VerdantScaleAffector](../Affectors/VerdantScaleAffector.html) attached.

For more information about fields in general, see the [Fields page](index.html). 

## Parameters
 
### Field Parameters

Changing any of the field parameters requires Verdant to replace the underlying render textures of the field. This can be very expensive and will almost certainly cause a frame hitch, so it should be avoided at runtime.

|:---------------|:--------------------------|
| `Resolution` | The resolution of the underlying field render textures, which along with Range determines the level of detail per square meter the field can handle. The [Debug Panel](../../UserGuide/DebugPanel.html) can be used to help visualize this. |
| `Wrap Mode` | The wrap mode of the underlying render texture, which determines if and how the field should be used for vegetation outside its range. Choose clamp if you primarily use the field for interactions and choose repeat if you primarily use it to add a base texture. Clamp will limit the field to its range whereas repeat will repeat the contents of the field throughout the world.  |
| `Range` | Can be thought of as the render distance of fields. It determines how far the field should stretch around the camera. Outside of this range affectors will have no effect. |
| `Set Shader Values Globally` | When this is set, all the shader values that Verdant uses will be made available as global shader variables. You need to enable it if you are using a Verdant shader on a regular material or if you want to read from this field in a custom shader. For details, check the page [Writing Custom Shaders]("../../UserGuide/WritingCustomShaders.html") |

### Base Texture Parameters

Fields can be initialized with a base texture. This texture will be repeated throughout the field and can be an excellent way to add some variance to vegetation. Affectors will paint over it (or, blend, depending on their settings), but when they are cleared the field will always go back to its base texture.

|:---------------|:--------------------------|
| `Base Texture` | The texture to use. |
| `Texture Frequency` | Sets how often the texture should repeat in X and Y throughout the field. |
| `Map From Grayscale` | When set the texture will be interpreted as a greyscale texture where black maps to Minimum Scale and white to Maximum Scale. |
| `Minimum Scale` | The minimum scale used for a remapped texture. |
| `Maximum Scale` | The maximum scale used for a remapped texture. |

### Restore Over Time

By default anything painted to the field will stay there as long as it is in range. These parameters let you set it up to fade back to its initial state. 
 
|:---------------|:--------------------------|
| `Restore Over Time` | If enabled this field will fade back to its base state over time. If a Base Texture is set it will restore to that, otherwise it will fade to scale 1. |
| `Restoration Rate` | The speed at which the field will be restored. |
| `Updates Per Second` | The frame rate at which the field will be restored. Verdant interpolates restoration, so it will always appear smooth even if the number is low. When restoration is enabled the update rate also becomes the rate at which affectors are sampled. Setting the number higher will result in smoother trails, especially on smaller objects, but is more performance intensive. |

## Public Methods

|:---------------|:--------------------------|
| `void SetParameters()` | Applies any parameters that have been changed. This can be an expensive operation depending on what was changed. |

