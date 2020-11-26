---
layout: default
title: "VerdantCamera"
parent: "Component Reference"
---


# VerdantCamera

The central component of Verdant responsible for rendering vegetation in the world. A camera without one of these won't be able to see any Verdant vegetation. Different cameras can have different settings and function independently. 

The camera generates a simplified representation of the world and uses it to place vegetation procedurally. This takes the form of several [Fields]. 

When rendering Verdant in the scene view the scene camera will base its settings of one of the VerdantCameras in the scene. If there is a main camera it will always use that one, otherwise the first found will be used. 

## Parameters

|                |                           |
|:---------------|:--------------------------|
| Render Distance | Lorem ipsum dolor sit amet |
| Coverage Modifier | Lorem ipsum dolor sit amet |
| Placement Mode | Lorem ipsum dolor sit amet |
| Smoothing Level | Lorem ipsum dolor sit amet |
| Override Layer | Lorem ipsum dolor sit amet |
| Layer | Lorem ipsum dolor sit amet |

### Precision Settings
#### Coarse HeightField Resolution
#### Detail HeightField Resolution
#### Detail Field Ratio
#### Culling Tile Resolution

### Twin Objects
#### Twin Replacement Threshold
#### Twin Position Retrieval Range

### Cloud Shadows
#### Cloud Shadows
#### Cloud Shadow Size
#### Cloud Shadow Strength

### Global Shader Data
#### Apply Cloud Shadows Globally
#### Global Wind Mode

## Public Methods
#### SetParameters()