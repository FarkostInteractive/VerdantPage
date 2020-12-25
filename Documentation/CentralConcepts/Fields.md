---
layout: default
title: "Fields"
nav_order: "0"
parent: "Central Concepts"
---

# Fields

## Infinity

Fields are what lets Verdant handle virtually infinite worlds.

To understand how VerdantCamera works, imagine a piece of cloth that spreads over your scene infinitely in the X and Z axises. The cloth exists as a mathematical formula, so it has essentially no memory footprint. VerdantCamera finds the part of the cloth in its view and cuts it out. It then presses the cut piece down and shapes it around the VerdantObjects in the scene. It cuts again around each object. Finally, it fills out the cloth that remains with instances of VerdantTypes at their individual densities. The rest of the cloth is not even considered and so has no performance impact at all, and since the cloth is infinite the camera could cut out its piece anywhere. The effect is that vegetation outside the view of the camera stops existing completely. There is no position, no buffer to clean up, to Verdant it is as if it never existed. Without a VerdantCamera, it cannot be seen. 
