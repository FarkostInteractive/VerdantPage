---
layout: default
title: "VerdantCamera"
parent: "Component Reference"
nav_order: "1"
---

# VerdantCamera

The central component of Verdant responsible for rendering vegetation in the world. A camera without one of these won't be able to see any Verdant vegetation. Different cameras can have different settings and function independently. 

The camera generates a simplified representation of the world and uses it to place vegetation procedurally. This takes the form of several [Fields]. 

When rendering Verdant in the scene view the scene camera will base its settings of one of the VerdantCameras in the scene. If there is a main camera it will always use that one, otherwise the first found will be used. 

## Parameters

|:---------------|:--------------------------|
| `Render Distance` | Lorem ipsum dolor sit amet |
| `Coverage Modifier` | Lorem ipsum dolor sit amet |
| `Placement Mode` | Lorem ipsum dolor sit amet |
| `Smoothing Level` | Lorem ipsum dolor sit amet |
| `Override Layer` | Lorem ipsum dolor sit amet |
| `Layer` | Lorem ipsum dolor sit amet |

### Precision Settings

|:---------------|:--------------------------|
| `Coarse HeightField Resolution` | Lorem ipsum dolor sit amet |
| `Detail HeightField Resolution` | Lorem ipsum dolor sit amet |
| `Detail Field Ratio` | Lorem ipsum dolor sit amet |
| `Culling Tile Resolution` | Lorem ipsum dolor sit amet |

### Twin Objects

|:---------------|:--------------------------|
| `Twin Replacement Threshold` | Lorem ipsum dolor sit amet |
| `Twin Position Retrieval Range` | Lorem ipsum dolor sit amet |

### Cloud Shadows

|:---------------|:--------------------------|
| `Cloud Shadows` | Lorem ipsum dolor sit amet |
| `Cloud Shadow Size` | Lorem ipsum dolor sit amet |
| `Cloud Shadow Strength` | Lorem ipsum dolor sit amet |

### Global Shader Data

|:---------------|:--------------------------|
| `Apply Cloud Shadows Globally` | Lorem ipsum dolor sit amet |
| `Global Wind Mode` | Lorem ipsum dolor sit amet |

## Public Methods

|:---------------|:--------------------------|
| `SetParameters()` | Lorem ipsum dolor sit amet |