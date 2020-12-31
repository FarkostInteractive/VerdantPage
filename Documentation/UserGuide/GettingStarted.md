---
layout: default
title: "Getting Started"
parent: "User Guide"
nav_order: "0"
---

# Getting Started

{: .no_toc }

This page will get you up and running with Verdant in your project.

## What is Verdant?

Chances are you already have some idea seeing how you're here, but I want to give a quick introduction regardless. Verdant is a system for rendering vegetation in Unity. It is a procedural system that excels at rendering dense fields in large worlds, be they carefully handcrafted or generated at runtime. When you work with Verdant you will rarely be placing anything directly. Rather, you’ll tell the system which parts of the world you want covered in vegetation. This gives you a bit less control than you might be used to, but also allows for unparalleled flexibility, speedy iteration and keeps your scenes and prefabs lightweight. You won’t need to wrangle any large data structures and the system never needs to load anything from disk. All while keeping a small, constant memory footprint, regardless of the size of your scene.

## Basic Setup

If this is your first time using the system, I suggest starting from a blank scene and going through the procedure once before trying it in your existing scenes. This will preemptively help you isolate any issues or conflicts you might have.

### Deferred Rendering
If you’re using deferred rendering, Verdant includes a special light model which adds translucency and opacity for vegetation. To set it up, go to Edit > Project Settings, then select Graphics and change the ’Deferred’ dropdown to ’Custom Shader’. Then select the shader Verdant-DeferredShading. You also need to set Deferred Reflections to Custom Shader and set its Shader field to Verdant-DeferredReflections. While there are cases where the the default model will look fine, many important Verdant features will be unavailable and some vegetation will be lit strangely unless you use the Verdant Shader.

![Image showing the Unity graphics settings with the Deferred popup open](Media/UnityGraphicsSettings.PNG "Setting the deferred shader")

If you are not using deferred rendering, you should! Verdant performs best in deferred, though it will do fine in forward too.

Once you’ve imported Verdant, go to your main camera and add the component ’VerdantCamera’. There’s a lot of settings here, but the defaults will serve you well for now.

![A GameObject with both a VerdantCamera and a Unity camera component](Media/VerdantCameraAdded.PNG "VerdantCamera added")

With the camera set up, choose an object in the scene or create a new box or plane to serve as your ground. Any ground-like object with a MeshRenderer will do. 

![A box that has been scaled up to (30,1,30) to act as a floor](Media/FlooringBox.PNG "Ground box")

Add the component ’VerdantObject’ to it. Then, under the list Verdant Types, add the included type ’VerdantNormalUpGrass’. You might need to click the eye icon in the upper right corner to see it.

Your VerdantObject component should now look like this:

![A VerdantObject configured with the simple VerdantNormalUpGrass type](Media/AddVerdantObject.PNG "VerdantObject parameters")

Make sure the object is in view of the camera. You should now be able to hit play and see something like the below!

![The previously added box covered in short green grass](Media/GroundWithGrass.PNG "Ground with vegetation")

You can also preview Verdant in the scene view. Go to the menu bar and select Verdant > Render in Editor. Try moving your VerdantObject around a bit and see how it updates in real time as you work. Be aware that the scene view preview will perform worse than a build, so it can be a little heavy on lower end systems.

![The system menu bar with the menu Verdant open and the option Render In Editor Selected](Media/MenuBar.PNG "Menu Bar")

Finally, the vegetation is looking a little bit stiff at the moment, so we should add some wind. Create a new GameObject and add the component VerdantWindVolume to it. If gizmos are enabled you will be able to see the volume as a blue box when it is selected. When the camera enters it a gentle breeze should rise through your field!  

## Your next steps

You've now tried the basics, but there are so many other things to discover in Verdant. Next we will take a look at the Verdant workflow to help you understand how to build scenes. [Continue on the Workflow page](Workflow.html). 
