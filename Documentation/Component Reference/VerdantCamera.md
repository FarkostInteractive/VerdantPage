---
layout: default
title: "VerdantCamera"
parent: "Component Reference"
nav_order: "1"
---

# VerdantCamera

The central component of Verdant responsible for rendering vegetation in the world. A camera without one of these won't be able to see any Verdant vegetation. Different cameras can have different settings and function independently. 

When rendering Verdant in the scene view the scene camera will base its settings of one of the active VerdantCameras. If there is a main camera it will always use that one, otherwise the first found will be used. 

## Parameters

|:---------------|:--------------------------|
| `Render Distance` | Determines the radius around the camera in which Verdant will render vegetation. Higher numbers will mean more instances being rendered, though the cost of that can be mitigated by using the falloff settings on your VerdantTypes.  |
| `Coverage Modifier` | A multiplier for vegetation density. Use it to change the amount of vegetation drawn at runtime for different graphics settings or to quickly test what your scene would look like with lower density. |
| `Placement Mode` | Determines how Verdant instances read the underlying heightfield. Sharp places each instance without any interpolation and gives you perfect edges but choppy artifacts on steep hills. Smooth interpolates placement and is perfect for large rollign hills, but can cause artifacts along edges between objects or sudden changes in height. Hybrid combines the two and will give you good results in almost all cases, but is slightly more expensive. If your world is very geometric, use Sharp. If it is primarily natural landscapes, use Smooth. If neither description really fits, use Hybrid. |
| `Smoothing Level` | Used to control how much smoothing the Hybrid Placement Mode allows. |
| `Override Layer` | If set lets you specify a render layer for all your vegetation. |
| `Layer` | The layer to use for Verdant if Override Layer is set. |

### Precision Parameters

Changing any parameters in this section will cause Verdant to create new field textures when SetParameters is called, which is expensive. You should avoid doing it at runtime.

These parameters control how precise Verdants' understanding of the world around it is. Most of the time there's no need to change them unless you have a very high or low Render Distance or are noticing artifacts in the world. If you do want to change them it's important to understand how they work.

Verdant uses two heightfield, one for nearby and one for distant vegetation. For each the minimum amount of detail is the world space size of one pixel. Details smaller than that will either be ignored or drawn more coarsely. How much detail you need depends on your scene. A world that mostly consists of smooth landscapes does not need as high a resolution as one consisting of lots of small sharp objects at unexpected angles.

The threshold between the two fields is determined by Detail Field Ratio. If your render distance is 200 and the Detail Field Ratio is 0.4 the detail field is used for the first 80 units. For most types of games the detail field is the more important of the two, so start by increasing its resolution if you're having problems. 

When gizmos are enabled and the Precision Parameters foldout is open you will see a large grid drawn around the camera. This grid illustrates the precision parameters and shows you how large each cell of the detail heightmap and the coarse heightmap are in world space. The chessboard pattern in the coarse grid area illustrates the size of the culling tiles. 

|:---------------|:--------------------------|
| `Coarse HeightField Resolution` | The resolution of the coarse heightfield, which is used to place distant vegetation.  |
| `Detail HeightField Resolution` | The resolution of the detail heightfield, which is used to place nearby vegetation.  |
| `Detail Field Ratio` | The ratio of the Render Distance at which the Detail Heightfield is replaced by the Coarse heightfield. |
| `Culling Tile Resolution` | Determines how large the culling zones for Verdant are. Higher numbers will mean more precise culling but make the culling process itself more expensive. Avoid increaing it unless you have artifacts or performance issues specifically related to how the scene is culled. |

### Twin Objects

|:---------------|:--------------------------|
| `Twin Replacement Threshold` | The distance at which Verdant will replace instances with Twin Objects if specified on the VerdantType. |
| `Twin Position Retrieval Range` | The range within which Verdant retrieves positions for twin replacement from the GPU. Must be larger than the Twin Replacement Threshold. Large ranges mean fewer transfers from GPU Memory, but makes each transfer more expensive. |

### Cloud Shadows

|:---------------|:--------------------------|
| `Cloud Shadows` | A repeating greyscale texture that is projected onto vegetation as if it were shadows cast by clouds. If there is wind in the scene it will scroll in the wind direction at a speed relative to the wind speed. |
| `Cloud Shadow Size` | Scales the shadow texture up or down. |
| `Cloud Shadow Speed` | A multiplier on the wind speed that controls how quickly the cloud texture scrolls. |
| `Cloud Shadow Strength` | Controls strongly the cloud shadows will be applied. |

### Global Shader Data
 
See [Writing Custom Shaders](../UserGuide/WritingCustomShaders.html) for more information about using Verdant data in your own shaders.

|:---------------|:--------------------------|
| `Apply Cloud Shadows Globally` | Sets the cloud shader parameters to global shader variables. Should be set if you're using a Verdant shader on a regular material or if you want to use Verdant data in a shader of your own. |
| `Global Wind Mode` | Sets wind data to global shader variables. As wind is calculated per type this works a little bit differently than other global shader variables. It can either be set to Global or Per Material. For Global you will need to specify which VerdantType to use to calculate the global value. Per material will set the Verdant value on the material you specify, and each material can have a VedantType of its own.  |

## Public Methods

|:---------------|:--------------------------|
| `void SetParameters()` | Needs to be called when any parameter has been changed to apply it. Depending on the changed parameters this can be expensive operation, so be careful if you're calling it frequently. |