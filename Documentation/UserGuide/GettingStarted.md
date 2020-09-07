---
layout: default
title: "Getting Started"
parent: "User Guide"
grand_parent: "Documentation"
nav_order: "2"
---

# Getting Started

This page will get you up and running with Verdant in your project.

## What is Verdant?

Chances are you already have some idea if you got this far, but I want to give a quick introduction regardless. Verdant is a system for rendering vegetation in Unity. It is a procedural system that excels at rendering dense fields in large worlds, both ones premade and generated at runtime. When you work with Verdant you will rarely be placing vegetation directly. Rather, you’ll tell the system which parts of the world you want covered in greenery. This gives you a bit less control than you might be used to, but also allows for unparalleled flexibility, speedy iteration and it keeps your scenes and prefabs lightweight. You won’t need to wrangle any large data structures and the system never needs to load anything from disk. All while keeping a small, constant memory footprint, regardless of the size of your scene.

## Basic Setup

If this is your first time using the system, I suggest starting  from a blank scene and going through the procedure once before trying it in your existing scenes. This will preemptively help you isolate any issues or conflicts you might have.

Once you’ve imported Verdant, go to your main camera and add the component ’VerdantCamera’. There’s a lot of settings here, but the defaults will serve you well for now.

[Image of Verdant Camera in editor]

With the camera set up, choose an object in the scene or create a new box or plane to serve as your ground. Any ground-like object with a MeshRenderer will do. Add the component ’Verdant Object’ to it. Then, under the list Verdant Types, add one of the included types. You might need to click the eye icon in the upper right corner to see them.

[Image of eye button]

Make sure the object is in view of the camera. You should now be able to hit play and see something like the below!

[Image of working Verdant]

You can also preview Verdant in the scene view. Go to the menu bar and select Verdant > Enable Rendering in Editor. Try moving your VerdantObject around a bit and see how it updates in real time as you work. 

[Image of menu bar and editor Verdant]

If you’re using deferred rendering, Verdant includes a special light model which adds translucency for vegetation. To set it up, go to Edit > Project Settings, then select Graphics and change the ’Deferred’ dropdown to ’Custom Shader’. Then select the shader Verdant-DeferredShading. While there are cases where the the default model will look fine, many important Verdant features will be unavailable and some vegetation will be lit strangely unless you use the Verdant Shader.

[Image of graphics settings and translucency]

Finally, the vegetation is looking a little bit stiff at the moment. Add a Verdant Wind Volume to the scene.

When the camera enters the volume a gentle breeze should rise through your field!  

## Your next steps

You now know the basics, but there are plenty of other things to discover in Verdant. Here are a few suggestions for where to go next based on your needs:

q. I’m having a problem with the setup
a. The [FAQ] or [forum thread] might have answers, otherwise you can [contact me]

q. How do I add my own types?
a. 

q. I want to make it prettier!
a. Check out Beautifying your scene page

q. What should my workflow look like? How do I best take advantage of the Verdant’s strengths?
a. [Working with Verdant] describes just that

q. How do I improve my performance? What should I keep in mind as I work?
a. 

q. I want my characters to interact with vegetation
a. This is what affectors are for! Have a look at the [guide for affectors] and the [affector component pages]

q. It’s not interactive enough!
a. You might want to try using [Mirror GameObjects], which let you replace vegetation with GameObjects when they’re in a certain radius. I’m hoping to support extending the affector system in a future update!

q. How do I write my own shader?
a. [Writing Custom Shaders] shows you how to include Verdant features in your own shaders

q. How does it all work?
a. [Central Concepts] has the answers!

q. Why can’t I place vegetation under other VerdantObjects?
a. This is a limitation of the system, and one baked into its fundamentals. You can learn a bit about why in [Fields], but in short it happens because of the priorities Verdant makes. It’s something I’m very much hoping to solve in a future release, but 