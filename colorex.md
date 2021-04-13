---
title: Colorex
description: Experiments with color
date: 2016-12-07
category: image
---

# Colorex

Inspired by [Images with all colors](https://codegolf.stackexchange.com/questions/22144/images-with-all-colors) and [Tweetable Mathematical Art](https://codegolf.stackexchange.com/questions/35569/tweetable-mathematical-art) I decided to make images with a computer.
This was not my [first try](https://github.com/Maumagnaguagno/Spriter), so this time I wanted colorful images for a change.

## Brownian motion
This is the most exciting one, based on [Brownian motion](https://en.wikipedia.org/wiki/Brownian_motion) the computer starts at a random position and moves to a random direction one step, checks if the current position is occupied by a blank pixel and draws a color.
The color is incremented after each successful blank is found, otherwise the process is repeated, with the position being restarted every time the current position is not blank or within the canvas.
Instead of using the full 24 bits [color space](https://en.wikipedia.org/wiki/Color_space) I decided to use 18 bits, 262144 colors, jumping a few visually equal colors along the way to make small images prettier.
Then generating a X mirrored image to result in something like a [Rorschach test](https://en.wikipedia.org/wiki/Rorschach_test) and have some fun seeing the computer testing my sanity.

![Brownian motion X mirrored](img/colorex_brownian.png "Now tell me, what do you see?")

## Simple formulas
Several cool images can be created by simple formulas that take into account only math functions, the X and Y coordinates and sometimes the canvas width and height.
The following images were created using an R8G8B8 representation, which means 8 bits for each color channel.
The functions used here are from Ruby and the channel values truncated to 8 bits.

### ``[rand(256), rand(256), rand(256)]``
The easiest one is just random noise, for each XY pixel draw any RGB color.

<canvas id=c0 width=1024 height=1024></canvas>

### ``[x * y, 0, 0]``
An alien texture created by multiplication of coordinates on the red channel.

<canvas id=c1 width=1024 height=1024></canvas>

### ``[x * y, y, Math.hypot(x,y).round]``
Slightly more interesting to see than the previous one.
The green channel varies according to Y while the circular effect is created by the hypotenuse.

<canvas id=c2 width=1024 height=1024></canvas>

### ``[x | y, x & y, Math.hypot(x,y).round]``

<canvas id=c3 width=1024 height=1024></canvas>

### ``[x * y, x & y, Math.hypot(x - width / 2, y - height / 2).round]``

<canvas id=c4 width=1024 height=1024></canvas>

### ``[y % 2 == 0 ? x & 1 : x, x & y, x * y]``

<canvas id=c5 width=1024 height=1024></canvas>

### ``[x | y, x & ~y, ~x & y]``

![5](https://user-images.githubusercontent.com/11094484/114533084-e84bc400-9c23-11eb-9180-32f691285c16.png)

### ``[5 * y, 15 * y, 255 * x / width]``

![6](https://user-images.githubusercontent.com/11094484/114533128-f7327680-9c23-11eb-8d7a-a60bcf367041.png)

### ``[r = x == 0 ? 0 : 5 * y % x, 3 * r, 255 * x / width]``
An intermediate value ``r`` is used to avoid division by zero errors.

![7](https://user-images.githubusercontent.com/11094484/114533120-f39eef80-9c23-11eb-87dd-fea270069116.png)

### ``[r = x.zero? ? 0 : 5 * y % x, 3 * r % 12, 255 * x / width]``
An intermediate value ``r`` is used to avoid division by zero errors.

![8](https://user-images.githubusercontent.com/11094484/114533126-f699e000-9c23-11eb-8343-c617a96e415d.png)

### ``[(a & b) * 2, (a + b) * 2, (a | b) * 2]``
Two intermediate variables are used, ``a = (x + 1) % (y + 1)`` and ``b = (y + 1) % (x + 1)``

![9](https://user-images.githubusercontent.com/11094484/114533194-06b1bf80-9c24-11eb-846b-d3fdfe7f57ae.png)

### ``[(r * r * 255).to_i, (g * g * 255).to_i, (b * b * 255).to_i]``
The color wheel uses a little bit more math, with ``PI066 = Math::PI * 2 / 3``, ``a = Math.atan2(y - height / 2, x - width / 2) / 2``, ``r = Math.cos(a)``, ``g = Math.cos(a - PI066)`` and ``b = Math.cos(a + PI066)``.

![Color wheel](https://user-images.githubusercontent.com/11094484/114532564-55128e80-9c23-11eb-8fce-61c6e15a41fb.png)

Eventually I am going to populate this page with more algorithms and pretty images, stay tuned.

<script>
const width = 1024, height = 1024, size = width * height * 4;
function random(value) {return Math.floor(Math.random() * value);}
function draw(index) {
  var ctx = document.getElementById('c' + index).getContext("2d");
  var image = ctx.createImageData(1024, 1024);
  var data = image.data;
  var x = 0, y = 0;
  for(var i = 0; i < size; i += 4) {
    switch(index) {
    case 0:
      data[i]   = random(256);
      data[i+1] = random(256);
      data[i+2] = random(256);
      break;
    case 1:
      data[i]   = x * y % 256;
      data[i+1] = data[i+2] = 0;
      break;
    case 2:
      data[i]   = x * y % 256;
      data[i+1] = y % 256;
      data[i+2] = Math.round(Math.hypot(x,y)) % 256;
      break;
    case 3:
      data[i]   = (x | y) % 256;
      data[i+1] = x & y % 256;
      data[i+2] = Math.round(Math.hypot(x,y)) % 256;
      break;
    case 4:
      data[i]   = x * y % 256;
      data[i+1] = x & y % 256;
      data[i+2] = Math.round(Math.hypot(x - width / 2, y - height / 2)) % 256;
      break;
    case 5:
      data[i]   = (y % 2 == 0 ? x & 1 : x) % 256;
      data[i+1] = x & y % 256;
      data[i+2] = x * y % 256;
      break;
    
    }
    data[i+3] = 255;
    x += 1;
    if(x == width) {
      y += 1;
      x = 0;
    }
  }
  ctx.putImageData(image, 0, 0);
}
draw(0);
draw(1);
draw(2);
draw(3);
draw(4);
draw(5);
</script>