---
layout: default
title: "VerdantColorField"
parent: "Fields"
grand_parent: "Component Reference"
nav_order: "1"
---

# VerdantColorField

Enables interacting with vegetation color by using [VerdantColorAffectors](../Affectors/VerdantColorAffector.html) on GameObjects. The color of the field is multiplied into the base color and texture of each vegetation instance. 

Color fields are white by default, but this can be changed by configuring a base texture. When set, the field will read from the base texture for each pixel and set its value before any affectors get drawn. As the field moves it will continue to seamlessly fill in new areas from the base texture. This can be a very useful way to add some subtle global variance to vegetation color.

By default, color fields do not restore over time. Affectors will leave colored marks permanently as long as they remain in the range of the field. To enable restoration, use the parameters under Restore Over Time. If a base texture is set it will restore towards that, otherwise it will simply return to white.

For more information about fields in general, see the [Fields page](index.html). 

## Parameters
 
### Field Parameters

Changing any of these parameters requires Verdant to replace the underlying render textures of the field. This can be very expensive and will almost certainly cause a frame hitch, so it should be avoided at runtime.

|:---------------|:--------------------------|
| `Resolution` | The resolution of the underlying field render textures, which along with Range determines the level of detail per square meter the field can handle. The [Debug Panel](../../AdvancedGuide/DebugPanel.html) can be used to help visualize this. |
| `Wrap Mode` | The wrap mode of the underlying render texture, which determines if and how the field should be used for vegetation outside its range. Choose clamp if you primarily use the field for interactions and choose repeat if you primarily use it to add a base texture. Clamp will limit the field to its range whereas repeat will repeat the contents of the field throughout the world.  |
| `Range` | The render distance of the field. It determines how far the field should stretch around the camera. Outside of this range affectors will have no effect. |
| `Set Shader Values Globally` | When this is set, all the shader values used by Verdant will be made available as global shader variables. You need to enable it if you are using a Verdant shader on a regular material or if you want to read from this field in a custom shader. For details, check the page [Accessing Verdant Data]("../../AdvancedGuide/AccessingVerdantData) |

### Base Texture Parameters

|:---------------|:--------------------------|
| `Base Texture` | The texture to use. |
| `Texture Frequency` | Sets how often the texture should repeat in world X and Z throughout the field. |
| `Map From Grayscale` | When set the texture will be interpreted as a greyscale texture where black maps to Minimum Color and white to Maximum Color. |
| `Minimum Color` | The minimum color used for a remapped texture. |
| `Maximum Color` | The maximum color used for a remapped texture. |

### Restore Over Time

|:---------------|:--------------------------|
| `Restore Over Time` | If enabled this field will fade back to its base state over time. If a Base Texture is set it will restore to that, otherwise it will fade to white. |
| `Restoration Rate` | The speed at which the field will be restored. |
| `Updates Per Second` | The frame rate at which the field will be restored. Verdant interpolates restoration, so it will always appear smooth even if the number is low. When restoration is enabled the update rate also becomes the rate at which affectors are sampled. Setting the number higher will result in smoother trails behind moving affectors, especially on smaller objects, but is more performance intensive. |

## Public Methods

|:---------------|:--------------------------|
| `void SetParameters()` | Applies any parameters that have been changed. This can be an expensive operation depending on what was changed. |

