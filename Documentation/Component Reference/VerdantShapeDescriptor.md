---
layout: default
title: "VerdantShapeDescriptor"
parent: "Component Reference"
nav_order: "3"
---

# VerdantShapeDescriptor



## Parameters

|:---------------|:--------------------------|
| `Shape` | Selects the shape of this descriptor. When set to This Mesh the mesh of the GameObjects MeshFilter will be used if one is available. Other Mesh will open a field on the component for adding a specific mesh. Map is a special mode meant for when you want to project a texture onto the environment and takes the form of a plane locked to the X and Z axises. Finally, Primitive will open a field for selecting one of a set of primitive meshes. |
| `Other Mesh` | (Shape set to Other Mesh) The mesh that will be used as the shape of this descriptor. |
| `Primitive` | (Shape set to Primitive) Allows you to select between Sphere, Pyramid and Cube as the shape.  |
| `Scale Factor` | Lets you scale the shape in each axis. Will be multiplied per component with the Transform Scale. |
| `Specify Submesh` | When set the mesh used will be the specific submesh selected by the Submesh Index. If false, the entire mesh will be used. |
| `Submesh Index` | The submesh to use. |

## Public Properties

|:---------------|:--------------------------|
| `ChosenMesh` | Returns the mesh that is the shape of this descriptor and has been selected using Shape, Other Mesh and Primitive. |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Marks this descriptor and the components attached to it as dirty so they will be redrawn next frame. Must be called for changed parameters to take effect. |

