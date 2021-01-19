---
layout: default
title: "Fields"
parent: "Component Reference"
nav_order: "5"
has_children: true
---

# Fields

Fields are simplified representations of the game world that Verdant uses to place and animate vegetation. The three in this category are designed to facilitate interaction between dynamic objects and Verdant vegetation. 

All field componenets have a range and a resolution. Together, these determine the precision of the field. As the range is increased you will need to increase resolution to maintain the same level of detail per square meter. Higher resolutions are more expensive, so try to find the lowest precision you can get away with. When the camera reaches the edge of a field it will  recenter it around the camera and anything drawn outside the range will be lost. Increasing the range will let the field remember more, but decreases precision. Depending on the needs of your game you will have to find a balance between large, high resolution fields that remember as much as possible and small, cheap fields that center on the player.

Field components should always be attached to the same GameObject as the VerdantCamera, and if there are multiple VerdantCameras in the scene all of them will need field components of their own. Otherwise their vegetation won't react to affectors.

Fields are connected to Affectors, which paint information into them. If there is no field component on the VerdantCamera there is nothing for the affectors to paint into, so they won't influence your vegetation. Similarly, if there is a field but no affectors the field will be in its default state and contribute nothing.

There is a certain performance cost associated with all fields. It is highly dependent on the range, resolution and number of affectors used, but even just adding a field component will make the shader take a slightly more expensive path. Make sure you only use fields that you actually need, and tweak their parameters carefully to the minimum requirements of your game. If you are having issues, try disabling your field components to see what effect they have on frame time.

Like most components in Verdant, field parameters are accessible from script as well as the inspector. As a rule they should not be changed at runtime or you will risk big framerate hitches as the field rebuilds, but they can be very useful for implementing things like graphics settings. It's a good idea to decrease range and resolution on lower settings if your game allows it.

Fields can be viewed and visualized in the [Debug Panel](../../AdvancedGuide/DebugPanel.html). This can be a helpful way to gain an understanding of your resolution and range or to understand how affectors are drawn into fields.

Fields are also a broader technical concept in Verdant. You can find more information about them under [Central Concepts - Fields](../CentralConcepts/Fields).