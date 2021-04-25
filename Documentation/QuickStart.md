---
layout: default
title: "Quick Start"
nav_order: "1"
---

# Quick Start

This is a whirlwind tour through Verdant that shows you how to set up its most important systems without delving into the details. You'll also find this guide included in the package as a Getting Started-PDF. If you just want to start working this is the quickest way to do it, though I highly recommend that you also read the main [User Guide](UserGuide.html) afterwards. It goes into much more detail, showcases features not covered here and gives valuable context to help make the system easier to work with long-term.

## Table of contents
{: .no_toc .text-delta }
1. TOC
{:toc}

## Setup

To start using Verdant you need to set up a camera and provide a surface for vegetation to grow on. **Start by selecting the camera in your scene and adding the [VerdantCamera](ComponentReference/VerdantCamera.html) component.**

![Screenshot showing the VerdantCamera inspector](Media_GettingStarted/VerdantCameraInspectorUseDeferred.png "The VerdantCamera inspector")

If you’re using the forward rendering path you’ll see an information box recommending that you use deferred rendering. If you’re using deferred or are switching to it, you’ll see a warning sign asking you to change the deferred shader. There’s a button right there which you can click to have Verdant take care of it all for you. With that, the camera is ready to go!

![Screenshot showing the VerdantCamera inspector asking you to apply the deferred shader](Media_GettingStarted/VerdantCameraInspectorDeferredShaderApply.png "The VerdantCamera inspector prompting the deferred shader to be swapped")

We now need something for the camera to render. In Verdant, the basic concept is that vegetation grows on Verdant Surfaces. **A mesh or terrain can be made into a Verdant Surface by adding the corresponding component. Vegetation will then be placed onto the surface at runtime.**

Add a simple cube to the scene and shape it into a floor object. You can also use an existing mesh if you already have something for your ground. Either way, select the object and add the [VerdantObject](ComponentReference/VerdantObject.html) component to it. This will tell Verdant to treat the mesh as a Verdant Surface.

![Screenshot showing a selected gameObject with the VerdantObject component added](Media_GettingStarted/VerdantObjectComponentAdded.png "A GameObject with VerdantObject")

On VerdantObject there’s a list called Types. This is where we specify what vegetation should grow on the object. Add an element to the list and select the [VerdantType](ComponentReference/DataTypes/VerdantType.html) Grass01. You may need to unmark the eye corner in the top right of the asset list, which hides assets in packages.

![Screenshot showing the VerdantObject inspector with the type Grass01 applied](Media_GettingStarted/VerdantObjectInspectorWithType.png "The VerdantObject inspector")

Make sure the camera is pointed at the VerdantObject. If you run the game now you should see that the object has been covered with grass! **You can also enable rendering vegetation in Scene View by going to Verdant in the menu bar and selecting Render in Editor**.

![Screenshot showing a VerdantObject Inspector with the type Grass01 applied as rendered in play mode](Media_GettingStarted/VerdantObjectFromCameraView.png "A VerdantObject with grass01 on it")

## Controlling Surface Coverage

Most of the time we don’t want an entire object to be covered in vegetation. You can control the coverage with a greyscale texture under Masking Texture. Fully black areas will have no vegetation placed on them, while grey values will be multiplied into the vegetation scale.

![Screenshot showing a VerdantObject Inspector with the type Grass01 and a mask texture applied](Media_GettingStarted/VerdantObjectWithMaskTexture.png "A VerdantObject with a mask texture")

You can also make whole VerdantObjects into masks. They will be applied to all the VerdantObjects below them and can be used both to change the scale and to add VerdantTypes. Make an empty GameObject and add a VerdantObject to it. Scroll up to the [VerdantShapeDescriptor](ComponentReference/VerdantShapeDescriptor.html) component that was added automatically and change the Shape to Map. Then, set the Mode to Mask on VerdantObject. When you set a texture on this mask it will be projected downwards onto all the surfaces below it.

![Screenshot showing a VerdantObject Inspector with the type Grass01. A mask VerdantObject is projecting onto it from above.](Media_GettingStarted/VerdantObjectWithMaskObject.png "A VerdantObject being influenced by a mask VerdantObject")

## Terrain

Using Verdant on terrain is very similar to using it on meshes, but you need to use a different component. Add the [VerdantTerrain](ComponentReference/VerdantTerrain.html) component to your terrain. You can add types here too and Mask VerdantObjects will work the same way they do with other surfaces. You can’t add a mask texture to the terrain, but each type you add is mapped to one of its layers. Use the index number to select which. 

![Screenshot showing a VerdantTerrain covered in grass with some lupins painted in near the camera](Media_GettingStarted/VerdantTerrainPainted.png "A VerdantTerrain with two types")

For more about VerdantObjects, masks and terrains, see [Workflow](UserGuide/Workflow.html).

## Wind

We can see the grass, but there’s not much going on with it yet. **To make it a bit more lively we should add a Wind Volume to the Scene.** Create another empty GameObject and add the [VerdantWindVolume](ComponentReference/VerdantWindVolume.html) component. You’ll see a blue box in the Scene View representing its bounds. Anytime the camera is inside it the settings on the volume will be applied to all the vegetation in the scene. 

![Screenshot showing a VerdantObject with grass blowing in the wind. The wind volume is visible around it.](Media_GettingStarted/VerdantObjectWithWindVolumeAndEditor.png "A VerdantObject with grass affected by wind")


For more about Wind, see… [Wind](UserGuide/Wind.html).

## Creating VerdantTypes

If you want to use your own vegetation assets you will need to create a new [VerdantType](ComponentReference/DataTypes/VerdantType.html). Start by right clicking in the Project View and selecting Verdant > VerdantType.

![On the left, a screenshot showing the dropdown menu for creating assets. Verdant > VerdantType is selected. On the right, a screenshot of the VerdantType inspector](Media_GettingStarted/VerdantTypeCreation.png "VerdantType creation")

**If you need a starting point or just want to modify one of the built-in types you can create a duplicate type from it in your project.** Select the VerdantType in question and scroll all the way down in its inspector to the button “Duplicate VerdantType”. Click it, then choose a name and select a path.

![Screenshot showing the bottom of the VerdantType inspector. There is an arrow pointing to a button that says "duplication"](Media_GettingStarted/VerdantTypeDuplicateButton.png "VerdantType duplication button")

For more about VerdantTypes, see [Types and Groups](UserGuide/TypesAndGroups.html).

## Affectors

It’s possible to interact with Verdant vegetation using [Affector](ComponentReference/Affectors.html) components. They allow you to push, color or scale vegetation in the shape of a mesh. 

![Screenshot showing a VerdantObject with grass. There are three spheres floating above it in a line. One is making the grass below it shrink, one is pushing it aside, and the last one is coloring it red](Media_GettingStarted/VerdantObjectWithAffectors.png "VerdantObject with affectors")

**To use an affector you must first add its matching Field component to the GameObject with the VerdantCamera.** Try adding a [VerdantDeflectionField](ComponentReference/Fields/VerdantDeflectionField.html) component.

![Screenshot showing the inspector of VerdantDeflectionField. It has been added to a GameObject with a VerdantCamera](Media_GettingStarted/VerdantDeflectionFieldInspector.png "VerdantDeflectionField inspector")

![Screenshot showing the hierarchy view "Create" dropdown. Verdant > Deflection Affector is selected](Media_GettingStarted/VerdantDeflectionAffectorCreateDropdown.png "VerdantDeflectionAffector creation")

Then, you can create a [Deflection Affector](ComponentReference/Affectors/VerdantDeflectionAffector.html), which will push away the vegetation around it. We’ll use a shortcut by right clicking in the hierarchy, then going to the Verdant subsection. Many common objects can be created quickly here, including Deflection Affectors. As always you can customize the shape by selecting a different mesh. Drag the new object onto the ground and notice how the grass bends around it.

![Screenshot showing a VerdantObject with grass on it. There is a deflection affector in the shape of a sphere on it pushing the grass away.](Media_GettingStarted/VerdantDeflectionAffectorOnVerdantObject.png "VerdantDeflectionAffector in action")

For more about affectors, see [Using Affectors](UserGuide/UsingAffectors.html).

## Where next?

As mentioned many times now, you should absolutely dig into the main documentation. It will both make you a better user and lead you to discover many features that we haven’t covered here.

If any particular section has caught your eye then feel free to explore their respective pages in the [Component Reference](ComponentReference.html) and [User Guide](UserGuide.html). You can also start at the beginning for a more thorough version of this guide. Either way, make sure you stop by at the [Performance section](UserGuide/Performance.html), which hasn’t been covered here at all. Finally, experiment! Verdant is made to be responsive and easy to work with. I’m sure many of us learned to use Unity by just messing around with stuff we found neat, and that approach will serve you well here too. 

I hope you enjoy using Verdant!