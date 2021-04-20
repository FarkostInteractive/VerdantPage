---
layout: default
title: "Quick Start"
nav_order: "1"
---

# Quick Start

This is a whirlwind guide through Verdant that touches on the most important systems without going into detail. Its contents are the same as the getting started-PDF included with the package. If you just want to start working this is the quickest way to do it, though I highly recommend that you also read the main [User Guide](UserGuide.html). It goes into much more detail, showcases features not covered here and gives valuable context to help you understand how the system works.

## Setup

To start using Verdant you need to set up a camera and provide a surface for vegetation to grow on. Start by selecting the camera in your scene and adding the VerdantCamera component.

If you’re using the forward rendering path you’ll see an information box recommending that you use deferred rendering. If you’re using deferred or are switching to it, you’ll see a warning sign asking you to change the deferred shader. There’s a button right there which you can click to have Verdant take care of it all for you. With that, the camera is ready to go!
We now need something for the camera to render. In Verdant, the basic concept is that vegetation grows on Verdant Surfaces. A mesh or terrain can be made into a Verdant Surface by adding the corresponding component. Vegetation will then be placed onto the surface at runtime.

Add a simple cube to the scene and shape it into a floor object. You can also use an existing mesh if you already have something for your ground. Either way, select the object and add the VerdantObject component to it. This will tell Verdant to treat the mesh as a Verdant Surface.
On VerdantObject there’s a list called Types. This is where we specify what vegetation should grow on the object. Add an element to the list and select the VerdantType Grass01. You may have to unmark the eye corner in the top right of the asset list, which hides assets in packages.
Make sure the camera is pointed at the VerdantObject. If you run the game now you should see that the object has been covered with grass! You can also enable rendering vegetation in Scene View by going to Verdant in the menu bar and selecting Render in Editor.

## Controlling Surface Coverage

Most of the time we don’t want an entire object to be covered in vegetation. You can control the coverage with a greyscale texture under Masking Texture. Fully black areas will have no vegetation placed on them, while grey values will be multiplied into the vegetation scale.
You can also make whole VerdantObjects into masks. They will be applied to all the VerdantObjects below them and can be used both to change the scale and to add VerdantType. Make an empty GameObject and add a VerdantObject to it. Scroll up to the VerdantShapeDescriptor component that was added automatically and change the Shape to Map. Then, set the Mode to Mask on VerdantObject. When you set a texture on this mask it will be projected downwards onto all the surfaces below it.

## Terrain

Using Verdant on terrain is very similar to using it on meshes, but you need to use a different component. Add the VerdantTerrain component to your terrain. You can add types here too and Mask VerdantObjects will work the same way they do with other surfaces. You can’t add a mask texture to the terrain, but each type you add is mapped to one of its layers. Use the index number to select which. 

For more about VerdantObjects, masks and terrains, see Workflow.
Wind

We can see the grass, but there’s not much going on with it yet. To make it a bit more lively we should add a Wind Volume to the Scene. Create another empty GameObject and add the VerdantWindVolume component. You’ll see a blue box in the Scene View representing its bounds. Anytime the camera is inside it the settings on the volume will be applied to all the vegetation in the scene. 
For more about Wind, see… Wind.

## Creating VerdantTypes

If you want to use your own vegetation assets you will need to create a new VerdantType. Start by right clicking in the Project View and selecting Verdant > VerdantType.

As you can clearly see there’s a lot going on in here. Start by setting your mesh and texture. Then put your type on a surface in the world so you can see what happens as you change it. You can figure it out by messing with the parameters, but you should definitely, definitely still read up on what they all do!

If you need a starting point or just want to modify one of the built-in types you can create a duplicate in your project. Select the VerdantType in question and scroll all the way down in its inspector to the button “Duplicate VerdantType”. Click it, then choose a name and select a path.
For more about VerdantTypes, see Types and Groups.

## Affectors

It’s possible to interact with Verdant vegetation using Affector components. They allow you to push, color or scale vegetation in the shape of a mesh. 
To use an affector you must first add its matching Field component to the GameObject with the VerdantCamera. Try adding a VerdantDeflectionField component.
Then, you can create a Deflection Affector, which will push away the vegetation around it. We’ll use a shortcut by right clicking in the hierarchy, then going to the Verdant subsection. Many common objects can be created quickly here, including Deflection Affectors. As always you can customize the shape by selecting a different mesh.Drag the new object onto the ground and notice how the grass bends around it.
For more about affectors, see Using Affectors.

## Where next?

As mentioned many times now, you should absolutely dig into the main documentation. It will both make you a better user and lead you to discover many features that we haven’t covered yet.

If any particular section has caught your eye then feel free to explore their respective pages in the Component Reference and User Guide. You can also start at the beginning for a more thorough version of this guide. Either way, make sure you stop at the Performance section, which hasn’t been covered here at all. Finally, experiment! Verdant is made to be responsive and easy to work with. I’m sure many of us learned to use Unity by just messing around with stuff we found neat, and that approach will serve you well here too. 

I hope you enjoy using Verdant!