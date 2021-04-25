---
layout: default
title: "VerdantShapeDescriptor"
parent: "Component Reference"
nav_order: "3"
---

# VerdantShapeDescriptor

Many components in Verdant have a presence in the game world of some sort. VerdantShapeDescriptor is the common component they use to define what shape they should have.

There are four different shape modes to choose between: 
* This Mesh: Looks for a MeshFilter on this GameObject and uses its mesh.
* Other Mesh: Lets you supply a mesh of your own. Useful for objects that don't need a MeshFilter, to use a lower resolution version of the main mesh, or when the object is part of a mesh hierarchy that is more complicated than a single renderer.
* Map: An XZ-aligned plane meant for projecting textures onto other surfaces. Use when the texture is more important than the shape.
* Primitive: Opens a short list of premade meshes. Good for prototyping and for simple affectors.

You can think of VerdantShapeDescriptor as the MeshFilter of Verdant. It's a simple component that supplies more complex systems with mesh data. It will be added automatically for components that require them, so there's little reason to do so on your own. The exception is when creating objects from script, in which case VerdantShapeDescriptor must be added before any components that require it. 

A single shape descriptor can be shared by many different components. For example, you could have a shape that is both a [VerdantColorAffector](Affectors/VerdantColorAffector.html) and a [VerdantDeflectionAffector](Affectors/VerdantDeflectionAffector.html).

## Parameters

|:---------------|:--------------------------|
| `Shape` | Selects the shape of this descriptor. When set to This Mesh the mesh of the GameObjects MeshFilter will be used if one is available. Other Mesh will open a field on the component for adding a specific mesh. Map is a special mode meant for when you want to project a texture onto the environment and takes the form of a plane locked to the X and Z axises. Finally, Primitive will open a field for selecting one of a set of primitive meshes. |
| `Other Mesh` | (Shape set to Other Mesh) The mesh that will be used as the shape of this descriptor. |
| `Primitive` | (Shape set to Primitive) Allows you to select between Sphere, Pyramid and Cube as the shape.  |
| `Scale Factor` | Lets you scale the shape in each axis. Will be multiplied per component with the Transform Scale. |
| `Specify Sub Mesh` | When set the mesh used will be the specific sub mesh selected by the Sub Mesh Index. If false, the entire mesh will be used. |
| `Sub Mesh Index` | The sub mesh to use. |

## Public Properties

|:---------------|:--------------------------|
| `ChosenMesh` | Returns the mesh that is the shape of this descriptor selected using Shape, Other Mesh and Primitive. |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Marks this descriptor and the components attached to it as dirty so they will be redrawn next frame. Must be called for changed parameters to take effect. |

