---
layout: post
title: Creation of a Modular Level Constructor
---

Modular level design kits is quite old, but really good idea. You create different objects (modules) for walls, corners, windows, doors, archs, etc., and they should connect to each other easily, without causing any problems or visual artifacts.

Lets discuss some key points on why modular kits are awesome:

- They are scaleable. You can have only 3 objects (inside corner, outside corner and straight wall) and create huge dungeon using only them.
- Easy to maintain. If you have a bug in one object, you can easily fix it and every instance of this object in your levels will be fixed.
- Easy to add more elements to the kit without breaking it,
- Since you can have limited amount of objects that allow you to build endless levels, you require less content, which makes development cheaper.
- Using the same object over and over again instead of multiple unique objects is good for the game’s performance. 
- Makes it easier to split tasks between different specialists. An artist creates assets and a level-designer uses them to create levels.

In theory creating a modular kit isn’t that hard. But in reality you need some time to wrap your mind around the concept and to learn how to create models that will combine with each other easily. And if you create multi-level kit, which includes ladders, bridges and other similar objects - the tasks appears to be even harder.

First step in creating a modular level-design kit is to work with grid. Depending on your unique project you first determine the size of single cell. In Ancient Rivals I use a box of 2x2 meters (roughly size of a person in heavy armor).
Usually you want to use rectangular grid. There are some variations of grid, but they require quite a bit of experience, not everybody can handle them.

So I suggest stick to rectangular grid and use a box as a step of said grid.

![Alt text](http://i.imgur.com/nOfDk35.png)

Using those cubes you first create placeholder modular elements. At this step you don’t care about details; all you want is nicely working modular kit. Try to avoid creating elements whos size is smaller than the grid’s step. Having such elements is an indicator that your kit has problems.

In order to make your kit work, here’s minimal amout of elements you need to have:

- Straight wall.
- Outside corner.
- Inside corner.
- Floor tiles in case you don’t want to have simple plane with tileable texture.

After you have done your placeholder kit using simple cubes, and after you have tested it in all kind of situations, like different room sizes, it is time to convert your cubic objects into real assets. Follow your placeholders closely.
Be sure your work is based on iterations. Don’t pick single asset and make it beautiful. Instead pick all of your assets at once, improve them a little, test how they work together. If everything is fine, improve them all a little further. Rinse and repeat untill you have solid modular kit ready.

If you follow your placeholders precisely and if you utilize your grid, you will have something like this:

![Alt text](http://i.imgur.com/GM7Orel.png)

![Alt text](http://i.imgur.com/RdyamtY.png)

And lastly one more note. Creation of good modular level-design kit is a close collaboration between an artist and a level-designer. They work together from very first cube. Even after the kit is completed, the dialogue between them should continue: how can the kit be improved? What can you do better next time?
