---
layout: post
title: Dynamic shadows (Unity, mobile)
---

Recently we have finished our own little realtime shadows system, which works absolutely fine on mobile devices. We would like to demonstrate it and our programmer also wrote a little bit about how he achieved this result.

![Alt text](http://i.imgur.com/3wQUWZS.jpg)

<iframe width="560" height="315" src="https://www.youtube.com/embed/TYAD1xqwf48" frameborder="0" allowfullscreen></iframe>

For those interested our programmer shared some thoughts on this technique:

Dynamic shadows in our game use simple Texture Mapping technique. We use a plain-color texture with white background instead of a Depth Field that’s commonly used in Shadow Mapping.

Shadows are blurred with the Poisson technique with eight filter taps (tap coordinates are calculated inside of the vertex shader to reduce the cost of dependent texture reads). It’s also possible to use four points instead (in order to increase performance). There were twelve points in the original solution from the Dungeon Siege II article in ShaderX3. Blur is off by default and can be enabled via graphics settings of the game. In general, low-end devices with smaller screen resolution perform these blur tasks better than more powerful devices with bigger screen and / or resolution.

Dynamic shadows also blend with static ones with a simple one-step calculation of normalized lightness values between two specified boundaries. The lower boundary fully rejects the dynamic shadow while the upper boundary fully receives it. Values between the boundaries are linearly interpolated. Most of the formulae factors can be precomputed (once for lightness settings), so this method works faster than branches in shaders and looks better than sharp edges yielded by simple one-step thresholds. 