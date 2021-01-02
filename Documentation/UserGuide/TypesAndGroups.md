---
layout: default
title: "Types and Groups"
parent: "User Guide"
nav_order: "2"
---

# Types and Groups

## Table of contents
{: .no_toc .text-delta }

Now that we know how to use VerdantObject and VerdantTerrain we can start to think about creating types to put on them. You might have noticed that the type list asks for VerdantInstantiables. This is a broad category that contains two different asset types:

## VerdantType

Think of VerdantType as a combination of a material and a prefab. It is the blueprint from which Verdant will create countless instances populating the world. It describes which mesh to use, the scale, how stiff it should be in wind, all the way up to things like how densely instances should be placed at different distances. Each type of foliage you want to add to the world, meaning each different flower, type of grass, bush, and so on will need its own VerdantType. In general you can think of one mesh as mapping to one VerdantType.

You create VerdantTypes as assets in the Project view. Right click or use the + button in the upper left and select Verdant > VerdantType. Open the newly created type in the inspector and you'll find an abundance of parameters to play with.

![Inspector image]()

The parameters are split into a few different categories that determine what they control. We'll focus on their broad purpose here as to not completely overstuff the guide. Many names will be familiar from other systems in Unity, and for everything else there is always the [component reference](../ComponentReference/DataTypes/VerdantType.html) which contains a full rundown of everything. 

If you've enabled rendering Verdant in the scene view you'll notice that instances of a type automatically update in real time as you change it.

### Density and falloff

The first section controls how dense the instances of this type should appear. The main parameter, Density, represents the amount of instances in a 1x1 unit square. The bar graph below it controls the falloff, which is how the density decreases further from the camera. Each bar controls one tenth of the render distance, the bar furthest left being closest to the camera. Falloff acts as a multiplier on the density, so if density is 100 and the second falloff bar is 0.5 the amount of instances will halve in the second tenth. You can usually get away with a pretty dramatic falloff without changing how your game looks. The preset modes Sharp and Exponential will work well in most cases, but there is also a custom mode that lets you adjust the bars manually if needed.

### LODs

Below the falloff graph there should be a wide horizontal box. It represents that the type you created has one level of detail, and that it's set to be used for all of the falloff stages (skipping the last one makes the cutoff a radius rather than a sharp edge). By adding more LODs we can make it so vegetation further from the camera uses different parameters, like disabling shadows or using a simpler mesh. You'll notice that most of the parameters below have a checkbox next to them, and that for the first LOD they are all checked. You can try creating another LOD by pressing the + button on the right, then click on the new box to select it. 

Here, all the checkboxes are unmarked. By marking one we can override that parameter with a new value. Unmarked parameters will be inherited from the nearest LOD that did mark them, which is why the first must mark everything. Most of the time later LODs only change one or two parameters, and this way there's no need to fill in parameters unless we explicitly need them.

LOD fade is the only LOD parameter that cannot be inherited. It controls how smoothly one LOD fades into the next. For later LODs you can usually leave it at zero, but it can be very useful for the first section or two where the player is very aware of pop in. Increasing it has a small performance penalty, but can help mask more dramatic LODing like taking away shadows and become a net win by doing so. Try increasing it if your LOD transitions are very obvious.

### Per Type parameters

Finally, the last section applies to the entire type. They allow us to set a rendering layer, adjust how randomly the instances are placed (when set to zero they are packed in a tight repeating pattern) and to configure a Twin Object. The last option makes Verdant replace instances of the type with a prefab when within a certain distance and allows you to interact with vegetation fully as you would any GameObject. Twin objects have a guide of their own in the [advanced section](../AdvancedGuide/UsingTwinObjects.html). 

### Setting up a type

With the broad descripitions of the parameters out of the way, let's have a look at using it all in an example. We'll set up a simple type and in the process focus in on some of the more important parameters. 
 
***********

### The VerdantType limit

An important thing to know about VerdantType is that all the types currently in range of the camera count towards a limit. You can have a maximum of 31 active types at any given moment. This says nothing of the scene itself, which could have any number of types, but of those only 31 can be in range of the camera at any given time. You'll stay far below this limit most of the time, each added type has a performance overhead so it is generally a good idea to not have too many anyway. If you do need more than 31 at once, the next asset type can help you get around the limit by combining some types.

## VerdantGroup

It's very common to have cases where we might want multiple types to always appear together. To achieve that we can use a VerdantGroup, which lets us tie up multiple types into a bundle that acts as single type. 

![VerdantGroup Inspector]()

Using VerdantGroup is very simple. You can create them from the same menu as VerdantType. They have their own falloff and density settings, but otherwise all you need to do is add the types you want to include. You'll notice that a number appears next to them as you do so. This is the ratio, which controls how much of the group density to use for this type. A group with density 100 could for example have flowers at 0.2 and grass at 0.8, which makes for 20 flower instances and 80 grass instances. The ratio can be set higher than 1 if needed, but you should try to keep it so all your types add up to 1 as good practice. It makes the total density of the group easier to keep track of and saves you from nasty performance surprises. 

VerdantGroup can be used anywhere you can use VerdantType. They will count as one group in almost all ways except one: Performance. We'll get into it more in the [performance section](Performance.html) of the guide, but it's important to stress here too. Even with groups you should be mindful that each type you add has a performance overhead. Also remember that unless you set your ratios right you might end up with the sum total of the densities of your types. If you combine three grass types with density 100 and don't change the ratio you will end up with density 300, with the predictable performance cost therein.


We've now gone over the tools for building your scene and the Verdant vegetation therein. There's still one obvious visual aspect left unexplored though, and we'll take a look at that next in the section on [wind](Wind.html). 