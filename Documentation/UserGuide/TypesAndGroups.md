---
layout: default
title: "Types and Groups"
parent: "User Guide"
nav_order: "2"
---

# Types and Groups
{: .no_toc }

Now that we know how to use VerdantObject and VerdantTerrain we can start to think about creating types to put on them. You might have noticed that the type list asks for VerdantInstantiables. This is a broad category that contains two different asset types: VerdantType and VerdantGroup.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## VerdantType

Think of VerdantType as a combination of a material and a prefab. It is the blueprint from which Verdant will create all its many instances populating the world. It describes which mesh to use, the scale, how stiff it should be in wind, and much more all the way up to things like how densely instances should be placed at different distances. Each type of foliage you want to add to the world, meaning each different flower, type of grass, bush, and so on will need its own VerdantType. In general you can think of one mesh as mapping to one VerdantType.

You create VerdantTypes as assets in the Project view. Right click or use the + button in the upper left and select Verdant > VerdantType. Open the newly created type in the inspector and you'll find an abundance of parameters to play with.

![Inspector image]()

The parameters are split into a few different categories that determine what they control. We'll focus on their broad purpose here as to not completely overstuff the guide. Many names will be familiar from other systems in Unity, and for everything else there is always the [component reference](../ComponentReference/DataTypes/VerdantType.html) which contains a full rundown of everything. 

If you've enabled rendering Verdant in the scene view you'll notice that instances of a type automatically update in real time as you change it.

### Density and falloff

The first section controls how dense the instances of this type should appear. The main parameter, Density, represents the amount of instances in a 1x1 unit square. The bar graph below it controls the falloff, which is how the density decreases further from the camera. Each bar controls one tenth of the render distance, the bar furthest left being closest to the camera. Falloff acts as a multiplier on the density, so if Density is 100 and the second falloff bar is 0.5 the amount of instances will be 50 per unit squared in the second tenth. You can usually get away with a pretty dramatic falloff without changing how your game looks. The preset modes Sharp and Exponential will work well in most cases, but there is also a custom mode that lets you adjust the bars manually if needed. Only use Linear if your camera has a very high vantage point.

### Levels of Detail

Below the falloff graph there should be a wide horizontal box. It represents that the type you created has one level of detail, and that it's set to be used for all of the falloff stages (skipping the last one makes the cutoff a radius rather than a sharp edge). By adding more LODs we can make it so vegetation further from the camera uses different parameters, like disabling shadows or using a simpler mesh. You'll notice that most of the parameters below have a checkbox next to them, and that for the first LOD they are all checked. You can try creating another LOD by pressing the + button on the right, then click on the new box to select it. 

Here, all the checkboxes are unmarked. By marking one we can override that parameter with a new value. Unmarked parameters will be inherited from the nearest LOD that did mark them, which is why the first must include all of them. Most of the time later LODs only change one or two parameters. This way there's no need to fill in parameters unless we explicitly need them.

LOD fade is the only LOD parameter that cannot be inherited. It controls how smoothly one LOD fades into the next. For later LODs you can usually leave it at zero, but it can be very useful for the first section or two where the player is very aware of pop in. Increasing it has a small performance penalty, but can help mask more dramatic LODing like taking away shadows. If doing so gets you a net performance win it's well worth increasing it.

### Per Type parameters

Finally, the last section applies to the entire type. They allow us to set a rendering layer, adjust how randomly the instances are placed (when set to zero they are packed in a tight repeating pattern) and to configure a Twin Object. The last option makes Verdant replace instances of the type with a prefab when within a certain distance and allows you to interact with vegetation fully as you would any GameObject. Twin objects have a guide of their own in the [advanced section](../AdvancedGuide/UsingTwinObjects.html). 

### Setting up a type

With the broad descripitions of the parameters out of the way, let's have a look at using it all in an example. We'll set up a simple type and in the process focus in on some of the more important parameters. Start by creating a new type by right clicking in the Project view or clicking the plus button. Navigate to Verdant and select VerdantType. Then click on the type you created to open it in the inspector.

The one thing all types need is a mesh, so we'll start there. Go to the mesh field and either select one from Verdant or one of your own models.

### A note about making vegetation meshes

Normally when using models in Unity you place them as a gameObject hierarchy that may contain one or more meshes. Since Verdant renders meshes directly it can only do one mesh at a time and loses any transforms present in the hierarchy. It's important when you're authoring meshes to remember that you must combine all separate pieces, apply your transforms so the mesh naturally has its origin at the base of the plant, and make sure it uses the Y axis as its up axis. Otherwise it will look off or not appear at all. To make it animate as nicely as possible you should also try to not let the height exceed one meter.

While we won't interact much with the scene here it's good if you have a VerdantCamera and VerdantObject out that you can test the type on. If you do, set the VerdantObject to only use your new type. It should appear with your chosen mesh!

Next, let's think about the density. It's set as 100 by default, but let's decrease it to 75. That will give us almost the same look but save us 25% of the instances. The falloff is set to Sharp by default and we have no reason to change that, though if you want to you could change it to . We should however add another LOD. Click the plus button, then grab the handle between the two LODs and slide it to the left so the first LOD only covers the first section. 

Click on LOD 0 to show its parameters. It would make sense to add a texture next, and we can easily do that by setting the Texture parameter. If the texture is in greyscale or we want to tweak it slightly we can use the color parameter. You can also take this opportunity to select LOD 1, enable its color parameter and change it to something wildly different than LOD 0. Then, zoom out from your VerdantObject. You'll clearly see where Verdant switches between the two LODs. If you go back to LOD 0 you can increase the LOD Fade and see how it smooths the boundary between the two. When you're done, just disable the color parameter on LOD 1 and set the LOD Fade back to 0 for now.

If you have a second LOD for your mesh go ahead and set it in the Mesh parameter. Otherwise we'll keep both for the aforementioned performance reasons and focus on the first one. Click on LOD 0 again to refocus it. Notice how there are some parameters that are greyed out. This happens because of two parameters towards the top, Shading Level and Enable Alpha. We'll discuss them more in the [Performance Guide](Performance.html), but for now know that they are used to exclude certain features to make the type faster to render. We'll leave them as they are for now.

Of the two columns of LOD parameters the left one should mostly look familiar from the Unity standard shader. Some of them, such as Translucency, need a bit more context for how and why to use them. We'll return to them in the guide [Visual Flair](VisualFlair.html), where we'll use them to embellish our scene to great effect. The right hand column though contains a lot of important Verdant-specific settings. These are called the Instance Properties and control placement and animation behaviour.

The first one you'll notice is Billboarding, which allows us to set how much the instances should rotate to face the camera. With some types increasing billboarding can help make the scene appear denser, though it can also backfire and make it look artificial. Try to set it to 1 to see the difference, then try a middle value. Setting it between 0 and 1 can be a good compromise that makes it so the type will never be seen directly from the side, but also not appear to follow the camera as it moves.

This is also where you find the Scale property. This is the basic scale of the type that can be influenced by other parts of Verdant, like the Scale setting on VerdantObject. Other Scale values will get multiplied into this one, so make sure you set it to a good baseline value for your mesh.

The following four have to do with [Affectors](UsingAffectors.html) and [Wind](Wind.html), so we'll save those for later. After them are a few that we'll return to when going in depth on the visuals. I do want to point out the shadow settings though. Set Cast Shadows to On and see how it changes your scene. You might want to tweak your light a little bit to really bring them out. While shadows look really nice we need to be careful when using them as they are very performance intensive. We'll use our second LOD here to make it so they're only visible when we're close to them. Go to LOD 1 and enable the Cast Shadows parameter, then make sure it's set to Off. As you back away now you should notice the shadows disappear. If it's very obvious when they drop out, go back to LOD 0 and increase LOD Fade until you get a smoother transition. 

Finally, we'll change one of the Type Parameters at the bottom. Try increasing Placement Randomness and see how it changes the pattern your type grows in. For a dense type like this one we only need to bump it up slightly to make it a little less repetitive. Usually the sparser a type is set up the clearer the pattern becomes, and the more you have to increase Randomness. 

### The VerdantType limit

An important thing to know about VerdantType is that all the types currently in range of the camera count towards a limit. You can have a maximum of 31 active types at any given moment. This says nothing of the scene itself, which could have any number of types, but of those only 31 can be in range of the camera at any given time. You'll stay far below this limit most of the time, each added type has a performance overhead so it is generally a good idea to not have too many anyway. If you do need more than 31 at once, the next asset type can help you get around the limit by combining some types.

## VerdantGroup

It's very common to have cases where we might want multiple types to always appear together. To achieve that we can use a VerdantGroup, which lets us tie up multiple types into a bundle that acts as single type. 

![VerdantGroup Inspector]()

Using VerdantGroups is very simple. You can create them from the same menu as VerdantType. They have their own falloff and density settings, but otherwise all you need to do is add the types you want to include. You'll notice that a number appears next to the types as you do so. This is the ratio, which controls how much of the group density to use for this type. A group with density 100 could for example have flowers at 0.2 and grass at 0.8, which makes for 20 flower instances and 80 grass instances. The ratio can be set higher than 1 if needed, but you should try to keep it so all your types add up to 1 as good practice. It makes the total density of the group easier to keep track of and saves you from nasty performance surprises. 

VerdantGroup can be used anywhere you can use VerdantType. They will count as one group in almost all ways except one: Performance. We'll get into it more in the [performance section](Performance.html) of the guide, but it's important to stress here too. Even with groups you should be mindful that each type you add has a performance overhead. Also remember that unless you set your ratios right you might end up with the sum total of the densities of your types. If you combine three grass types with density 100 and don't change the ratio you will end up with density 300, with the predictable performance cost therein.

We've now gone over the tools for building your scene and the Verdant vegetation therein. There's still one obvious visual aspect left unexplored though, and we'll take a look at that next in the section on [wind](Wind.html). 