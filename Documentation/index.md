---
layout: default
title: "Home"
nav_order: "0"
---

# Home

Welcome! 

The Verdant Documentation is split into four parts. There’s the user guide, which will offer you everything you need to get started and build beautiful, performant scenes. The Getting Started page will help you set up your first scene with some basic vegetation. Every subsequent page delves deeper into the various systems and how they relate to each other. 
The guide also contains some more general advice for how to best use Verdant, maximize performance and get your scene looking as pretty as possible. 

## [User Guide](UserGuide/index)

If you want to delve below the surface, Central Concepts goes into detail about how Verdant works. The User Guide often links to these pages, and I highly recommend giving them a quick read whenever it does so. Many of the design elements of Verdant will make more sense if you understand the systems they interface with. Central Concepts is meant to be understandable regardless of your technical background and offer a high level survey, not to overwhelm you with technical jargon. 

[Central Concepts Table]

The Component Reference explains every available Verdant Component in detail. This is where to look for information about specific parameters, what makes one component different from another, and so forth. It is also where you’ll find the scripting reference for each component. 

[Component Reference]

Finally there is the FAQ, which is always growing and changing. I try to include everything I can think of here, but as with any complex system there are many dark corners of Verdant which even I have yet to explore. If you have unanswered questions you can use the contact form to notify me and I will get back to you as soon as possible! 

There is also the Unity Forums Thread, which is likewise always growing. While I try to incorporate as much as I can into the FAQ, if you can’t find answers here it’s very worth looking through the thread anyway. It’s also a good way to get in touch with both me and other experienced users.

Here are a few suggestions for where to go next based on your needs:

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