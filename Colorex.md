---
layout: default
title: Colorex
description: Experiments with color
category: image
---

# Colorex

Inspired by [Images with all colors](http://codegolf.stackexchange.com/questions/22144/images-with-all-colors?noredirect=1&lq=1) and [Tweetable Mathematical Art](http://codegolf.stackexchange.com/questions/35569/tweetable-mathematical-art) I decided to make images with a computer.
This was not my [first try](https://github.com/Maumagnaguagno/Spriter), so this time I wanted colorful images for a change.

## Brownian motion

This is the most exciting one, based on [Brownian motion](https://en.wikipedia.org/wiki/Brownian_motion) the computer starts at a XY position and moves to a random direction one step, checks if the current position is occupied by a blank pixel a draws a color.
The color is incremented after each successful blank is found, otherwise the process is repeated, with the position being restarted every time the current position is not blank or within the canvas.
Instead of using the full 24 bits [color space](https://en.wikipedia.org/wiki/Color_space) I decided to use 18 bits, jumping a few colors along the way to use more colors in small images.
Then generating a X mirrored image to result in something like a [Rorschach test](https://en.wikipedia.org/wiki/Rorschach_test) and have some fun seeing the computer testing my sanity.
If you do the math ``2**18 == 512**2`` which means the image below could be 1024x512 since it is mirrored, but since bytes overflow and we can have bigger images, in this case a 1024 by 1024.

![Brownian motion X mirrored](img/colorex_brownian.png "Looks like a Rorschach test!")

Eventually gonna populate this page with more algorithms and pretty images, stay tuned.