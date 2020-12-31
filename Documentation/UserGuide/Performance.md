---
layout: default
title: "Performance"
parent: "User Guide"
nav_order: "6"
---

* If you can, I highly recommend using deferred lighting. In forward rendering Verdant will be rendered multiple times for each light, which can get very expensive.
* Lights with shadow casting will always cause Verdant to render a shadow pass one time per light. Be extra careful with these, and if you don’t need them you can disable shadows completely on your Verdant Type or limit them to the first LOD. On low performance systems this can sometimes double your framerate!