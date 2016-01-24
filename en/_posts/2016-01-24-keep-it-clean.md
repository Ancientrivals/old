---
layout: post
title: On keeping Unity project clean
---

In game development there are lots of little things that can cause lots of problems if they get out of hand. In this little article I’ll share some thoughts on how to keep your Unity (or any other, really) project clean. During my (Artem, project coordinator and technical artist speaking) days doing freelance I’ve seeт numerous projects and how they organize their files. It might seem like something of very little importance, but it does matter.

When working with Unity, it is common to use assets from their Store once in a while. You might want to import some models for prototyping or maybe a plugin to help you with in-app purchases. Anyway, all those unitypackages get decompressed and extracted straight into your project’s root folder. I’ve seen numerous times projects with dozens of such folders creeping out of control, making it hard to navigate around. By the time real problems arise, the projects becomes too big and it is kind of scary to move all those folders, because you can easily break something.

At the end of the day it is much easier to make decent folder structure when you are just creating new project. Among others, I like to make two important folders “Imported_Systems” and “Imported_Graphics”.

“Imported_Systems” is for plugins and tools. For instance, there could be placed IAP plugin or analytics system.

“Imported_Graphics” contains art assets, like environment models or maybe icons. Note that art created by your own artists is placed in different folder. I prefer to separate imported graphics because they do not obey established in your studio art pipeline. Sure, if you have time you can sort it out and convert imported stuff to meet all your pipeline guidelines.

While we are talking about keeping your project organized, I would like to suggest getting one of those build analyzers from Asset Store. Will cost you like 10 bucks, but will be super useful. It gives you detailed information on which assets are not used in the build, which are used and how many times. It is particularly helpful to track situations like the following: imagine you have 50 dungeon levels and 2 somewhat similar columns you use while building environments. First column is used 650 times, second one is used 25 times. It makes you think if you need that less used column at all, or maybe one of your level designers doesn’t know second column exists? Anyhow, it gives you information to help with various decisions.

So, analyzing game builds from time to time will help you to keep track record on which assets you actually use, and which are simply garbage waiting to be dealt with. And keeping your folders clean will reduce chaos when there will be many people contributing to the project.