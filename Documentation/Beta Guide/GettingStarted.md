---
layout: default
title: "Getting Started"
nav_order: "0"
parent: "Beta Guide"
---

# Welcome!
I'm so happy you're interested in testing Verdant! This page will take you through the process of getting everything set up.

## Setup
To use Verdant, you first need to have Unity installed. Any version from 2019.3 onward should work. If you already have a compatible version, use that! Otherwise, install the latest 2019 version or any 2020 version you'd like. A spread of different versions is good, it helps us cover more debugging ground. You can find them here: https://unity3d.com/get-unity/download.

Once you have a Unity version downloaded, start a new project (or open an existing one if you have something specific you want to use Verdant for). When the project has loaded, go to Windows > Package Manager. There's a plus symbol in the uppper left corner. Click that and select Load Package From Disk. Navigate to the folder where you extracted Verdant and find the file packagemanifest inside. Select it and wait for everything to import.

Finally open Windows > Console. If there are no errors in red you are ready to start using Verdant!


## First steps
We'll start with a very simple scene just to make sure everything's working. Create a cube and scale it up in X and Z. This will be our ground. 

Then, add the component VerdantCamera on the camera. You can leave all the settings on default.

On the ground object, add the component VerdantObject. In the Types list, set the length to 1 and select the type VerdantNormalUpGrass. This will make it so the specified Verdant type appears all over the object.

Finally, go to Verdant > Render in Editor. Your cube should be covered in grass.

## The Verdant Workflow
You might have noticed that Verdant works has a slightly different workflow from most vegetation systems. The primary interaction in Verdant is defining zones, rather than painting instances onto surfaces. You add VerdantObject to any objects you want vegetation 

 