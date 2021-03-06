---
layout: default
title: "VerdantScaleField"
parent: "Fields"
grand_parent: "Component Reference"
nav_order: "2"
---

# VerdantScaleField

Enables interacting with vegetation scale by using [VerdantScaleAffectors](../Affectors/VerdantScaleAffector.html) on GameObjects. The scale of the field is multiplied into the scale from the [VerdantType](../DataTypes/VerdantType.html) and from the [VerdantObject](../VerdantObject.html) of each vegetation instance. 

Scale fields are set to one by default, but this can be changed by configuring a base texture. When set, the field will read from the base texture for each pixel and set its value before any affectors get drawn. As the field moves it will continue to seamlessly fill in new areas from the base texture. This can be a very useful way to add some subtle global variance to vegetation scale.

By default, scale fields do not restore over time. Affectors will change scale permanently as long as they remain in the range of the field. To enable restoration, use the parameters under Restore Over Time. If a base texture is set it will restore towards that, otherwise it will simply return to scale one.

For more information about fields in general, see the [Fields page](index.html). 

## Parameters
 
### Field Parameters

Changing any of the field parameters requires Verdant to replace the underlying render textures of the field. This can be very expensive and will almost certainly cause a frame hitch, so it should be avoided at runtime.

|:---------------|:--------------------------|
| `Resolution` | The resolution of the underlying field render textures, which along with Range determines the level of detail per square meter the field can handle. The [Debug Panel](../../AdvancedGuide/DebugPanel.html) can be used to help visualize this. |
| `Wrap Mode` | The wrap mode of the underlying render texture, which determines if and how the field should be used for vegetation outside its range. Choose clamp if you primarily use the field for interactions and choose repeat if you primarily use it to add a base texture. Clamp will limit the field to its range whereas repeat will repeat the contents of the field throughout the world.  |
| `Range` | The render distance of the field. It determines how far the field should stretch around the camera. Outside of this range affectors will have no effect. |
| `Set Shader Values Globally` | When this is set, all the shader values used by Verdant will be made available as global shader variables. You need to enable it if you are using a Verdant shader on a regular material or if you want to read from this field in a custom shader. For details, check the page [Accessing Verdant Data]("../../AdvancedGuide/AccessingVerdantData.html") |

### Base Texture Parameters

|:---------------|:--------------------------|
| `Base Texture` | The texture to use. |
| `Texture Frequency` | Sets how often the texture should repeat in world X and Z throughout the field. |
| `Map From Grayscale` | When set the texture will be interpreted as a greyscale texture where black maps to Minimum Scale and white to Maximum Scale. |
| `Minimum Scale` | The minimum scale used for a remapped texture. |
| `Maximum Scale` | The maximum scale used for a remapped texture. |

### Restore Over Time
 
|:---------------|:--------------------------|
| `Restore Over Time` | If enabled this field will fade back to its base state over time. If a Base Texture is set it will restore to that, otherwise it will fade to scale 1. |
| `Restoration Rate` | The speed at which the field will be restored. |
| `Updates Per Second` | The frame rate at which the field will be restored. Verdant interpolates restoration, so it will always appear smooth even if the number is low. When restoration is enabled the update rate also becomes the rate at which affectors are sampled. Setting the number higher will result in smoother trails, especially on smaller objects, but is more performance intensive. |

## Public Methods

|:---------------|:--------------------------|
| `void SetParameters()` | Applies any parameters that have been changed. This can be an expensive operation depending on what was changed. |

