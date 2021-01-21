---
layout: default
title: "Workflow"
parent: "User Guide"
nav_order: "1"
---

# Workflow
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

This guide takes you through the process of building a scene with Verdant. 

## Surfaces and zones
As previously mentioned, the main usage of Verdant is defining zones for vegetation to grow in. In Getting Started we did this by adding the component VerdantObject to a box, which made grass appear on top of it. In the background adding VerdantObject made Verdant register the mesh of the box as a surface tracked within its systems. By then adding a type that surface was defined as a zone where VerdantNormalUpGrass grows. 

![Zone surface image]()

In short, a mesh recognized by Verdant is a surface, and on top of a surface we can have a zone of one or more types of vegetation. 

If you are used to thinking of vegetation as instances you paint onto geometry it's worth really internalizing this model. Think of VerdantObject as draping a piece of cloth over the mesh. The 'cloth', or the Verdant surface, is the only thing intrinsic to that object. It is not until the surface is observed by a camera that anything actually gets placed on it. 

Shaping these zones and surfaces is what we'll be using everything in this guide for. It's a very different model, but one that lets us do a number of interesting things. Let's have a look at some of them!

## VerdantObject Itself

We already went over it a bit earlier, but before we move on to more complex uses let's take a moment to look closer at the basics. VerdantObject is the component you add to a gameObject when you want vegetation to grow on it. It has a multitude of parameters, but the most important ones are the Type List and the Scale, which controls the types of vegetation growing on the object and their scale respectively.  

VerdantObject requires the gameObject to have a VerdantShapeDescriptor, and it will automatically add one if it's not already present. You can think of VerdantShapeDescriptor as the MeshFilter of Verdant, a very simple component that other components rely on to provide them with mesh data. Unlike MeshFilter we have a few different options to pick from for where that data comes. We can choose between them using the Shape dropdown. By default the shape descriptor looks for a MeshFilter on the gameObject and uses its mesh, but we can also set it to another mesh, a plane locked to the XZ axises (Map) or one of a number of simple shapes (Primitive).  

## Masking

Most scenes are more complex than a box with some grass on it. Even for scenes where that is enough we might want to make the grass a bit patchy or add some nice scale variance. We can use masking techniques to achieve this.

![Masked box]()

The simplest way to add a mask is to use the Scale Mask parameter on VerdantObject. You'll find it under the Masking Texture dropdown. This parameter takes a greyscale texture and uses it to scale the vegetation on the object. Black being zero and white being one, the value is multiplied into the scale set by the Scale parameter. 

The other parameters can be used to adjust the mask in a number of ways. You'll find in depth explanations in the VerdantObject component reference, but for now it's worth pointing out Min Scale and Max Scale. These let you change what values black and white map onto. For example, setting Min Scale to 0.4 makes that the new lowest value, so the texture will range from 0.4 to 1.0. Changing them can have quite significant effects on the character of your mask. 

![Gentler masked box]()

### Mask objects

Scale masks are well and good for simple uses, but there are more powerful tools in our belt. If the ground consists of several different objects and we want a consistent pattern or shape across all of them assigning individual scale masks become very cumbersome. 

![Mask object]()

It is better to use a single mask object. We can create these from a VerdantObject by changing its Mode parameter from Surface to Mask. In Mask mode the object can only be used to create zones on other surfaces, as the object itself is no longer a surface. Think of it as projecting the mesh downward onto the VerdantObjects below.

![Mask object w texture]()

The simplest usage is to just project the shape of the mesh, but we can also add a scale mask texture onto it. The texture will be projected along with the mesh, effectively giving us a way to apply a scale mask to several objects at once.

All the same parameters apply as on a regular scale mask, but there is one unique to mask objects worth pointing out. Setting Type Only makes it so the mask won't scale vegetation below it. Instead, it will simply add its types into it. It will do this anywhere its scale texture is not zero. This enables masks to add vegetation in shapes defined by scale masks without affecting what is already there.

![Type only mask]()

### Painted masks

If a normal scale mask isn't working for you Verdant includes tools for painting a mask yourself. These tools are more powerful in that they also allow you to paint different types directly into the mask. There is a full [Guide to painted masks](../AdvancedGuide/PaintingMaskTextures.html) in the advanced guide section. Just like scale masks painted masks can be used on VerdantObjects in both modes.

## Terrains

VerdantObject is specifically meant to be used for meshes, but it is not the only way to create surfaces. You can also use VerdantTerrain to create them from terrains. These work mostly the same as VerdantObject, with the exception that you can link types to different terrain layers. This allows you to, for example, easily add grass onto a grass texture you've already painted in. Just like VerdantObject you can always project a mask onto a terrain, even if it doesn't have types of its own.

## Building a Scene

We'll use everything discussed so far to put together a little scene. This part picks up from the setup done in [Getting Started](GettingStarted.html), so make sure you read that first.

Start by creating a terrain. To keep our example workable we need to shrink it down a bit, so go to the Terrain component and find the parameter Terrain Width and Terrain Length under Mesh Resolution in the settings. Set them both to around 50.

Go ahead and sculpt your terrain to make it more interesting. Once you're done, add a VerdantTerrain component to it. Also make sure your camera has a VerdantCamera. Then, on the VerdantTerrain, add a type by selecting one in the field marked 'Add'. In the example I'm using VerdantNormalUpGrass. Set the layer index of the type to 1. If you've enabled Render in Editor then you can also mark 'Update automatically' before you go back to the terrain. 

In the brush section choose Paint Texture and create two layers. The first will automatically fill the terrain. The second is the one we've mapped to our Verdant Type, so paint in that one however you like. If you enabled 'Update automatically' you'll see your changes instantly, otherwise you'll need to click the 'Update Terrain' button and/or run the game.  

We could add more types to the terrain directly, but we'll try doing it with a mask instead. Create an empty gameObject and add VerdantObject to it. On the VerdantShapeDescriptor that gets added automatically select Map as the Shape and increase the Scale Factor by about 10 to 20 in X and Z. Then set the VerdantObject Mode to Mask. We'll also mark Type Only. The mask will be used to add a patch of flowers into the field, so there's no need to change the scale. Then add a different type than the one used on the terrain (I'm using *****). Finally, open the Masking Texture dropdown and set a texture as a Scale Mask. You can use the included dsfkdsf or any similar greyscale texture with a clear bright shape. You'll now see the type you selected projected on the ground!

If we don't enable Type Only we get the result below, which is clearly not what we wanted here.

We'll also add a few VerdantObjects onto the terrain. Find a mesh you like, either of your own or one of the ones included with Verdant. Put it out into the world and add VerdantObject to it. Pick a third vegetation type and add it to its list. We'll add a scaling mask on this one too, so go into the masking options again and select a noise texture this time, like the included ******. You'll notice the vegetation becoming a little bit more varied.

To top it all off we'll apply a mask like that to all our vegetation. Create a new empty gameObject and add VerdantObject again. Set the VerdantShapeDescriptor to Map, increase the scale to cover the terrain and set the VerdantObject Mode to Mask. Add the noise as the scale mask on this one too. We'll increase the Min Scale to 0.7 so the mask only adds some subtle variation. You can then experiment with the scale, UV settings and transform to get a result you like. I rotate mine a bit and increase the UV scale. Regardless, you'll see that unlike our other mask the scale gets multiplied into all the existing vegetation.

## Wrapping Up

The components we've covered here are the core of Verdant and you'll return to them many, many times as you use it. Hopefully this guide has given you a sense of what that might look like and how you'll sculpt your scenes using surfaces and masks. There are many parameters we haven't touched on at all, and if you're interested in them the [Component Reference](../ComponentReference/Index.html) contains in depth guides to both [VerdantObject](../ComponentReference/VerdantObject.html) and [VerdantTerrain](../ComponentReference/VerdantTerrain.html). The next step will be adding your own types to the scene, which we'll cover in [Types And Groups](TypesAndGroups.html)!  