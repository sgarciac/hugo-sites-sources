---
title: PixelArt fun
author: sergio
type: post
date: 2011-07-12T14:54:41+00:00
url: /2011/07/12/pixelart-to-vector/
categories:
  - programación

---
I recently stumbled upon [this neat algorithm][1] by <a href="http://research.microsoft.com/en-us/people/kopf/" target="new">Johannes Kopf</a> and <a href="http://www.cs.huji.ac.il/~danix/" target="new">Dani Lischinski</a>. In short, their algorithm takes a pixelart image, such as  those found in 8-bit video games, and produces a new image that somehow makes you think &#8220;hey, that looks like what the artist actually had in mind&#8221;. In other words, it tries to generate the ideal image for which the pixel art is only an imperfect representation. The results explain it better:

<div id="attachment_435" style="width: 310px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/mario-comparison.png"><img class="size-medium wp-image-435  " src="http://personal.network.crazyrobot.net/files/2011/07/mario-comparison-300x155.png" alt="Mario - before, after" width="300" height="155" /></a>
  
  <p class="wp-caption-text">
    Mario from Super Mario Bros 3, using my own implementation. Click to zoom.
  </p>
</div>

If you are interested in this kind of stuff, I really recommend you read their paper, it is well written, easy to understand even for people with no deep knowledge of computer graphics (like myself) or for people with no programming training. I **partially** implemented their algorithm using Common Lisp and Zach Beane&#8217;s [Vecto][2] library, here&#8217;s a very very short description, just to illustrate the different steps. Some eye-candy at the end!

**Finding Vicinities**

First, we find which regions of the original image are part of a single feature.

<div id="attachment_440" style="width: 300px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/mario-vecinities.png"><img class="size-medium wp-image-440 " src="http://personal.network.crazyrobot.net/files/2011/07/mario-vecinities-290x300.png" alt="" width="290" height="300" /></a>
  
  <p class="wp-caption-text">
    Different regions are found. Notice the blue cross near the center of the image, where we need to decide which diagonal should be kept as part of a single feature
  </p>
</div>

**Re-shaping Pixels**

Once different regions have been found, pixels are re-shaped so that each region has a well defined contour.

<div id="attachment_443" style="width: 300px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/matio-voronoi.png"><img class="size-medium wp-image-443" src="http://personal.network.crazyrobot.net/files/2011/07/matio-voronoi-290x300.png" alt="" width="290" height="300" /></a>
  
  <p class="wp-caption-text">
    Each pixel is re-shaped to a voronoi cell calculated using the center point of each cell and points defined by its vicinities
  </p>
</div>

**Smoothing Edges**

Finally, edges are identified &#8230;

<div id="attachment_446" style="width: 300px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/mario-borders.png"><img class="size-medium wp-image-446" src="http://personal.network.crazyrobot.net/files/2011/07/mario-borders-290x300.png" alt="Edges are identified" width="290" height="300" /></a>
  
  <p class="wp-caption-text">
    Edges are identified
  </p>
</div>

&#8230; and smoothed

<div id="attachment_447" style="width: 310px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/mario-smooth.png"><img class="size-medium wp-image-447" src="http://personal.network.crazyrobot.net/files/2011/07/mario-smooth-300x142.png" alt="Smoothing" width="300" height="142" /></a>
  
  <p class="wp-caption-text">
    Smooth edges. Have a look at Mario&#8217;s eyes.
  </p>
</div>

Keep in mind that this is only a partial implementation, the original algorithm also re-shapes edges to make them look more natural, reducing zig-zag curves for example. (they also use a different type of curve, _b-splines_, which works better than what I&#8217;m using, _catmull rom_) and therefore produces much better results. Have a look at their examples in the linked article.

**Some Examples!**

Click on the images to enlarge

<div id="attachment_469" style="width: 553px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/spartan.png"><img class="size-full wp-image-469 " src="http://personal.network.crazyrobot.net/files/2011/07/spartan.png" alt="" width="543" height="372" /></a>
  
  <p class="wp-caption-text">
    Spartan
  </p>
</div>

<div id="attachment_450" style="width: 538px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/dk.png"><img class="size-full wp-image-450    " src="http://personal.network.crazyrobot.net/files/2011/07/dk.png" alt="" width="528" height="593" /></a>
  
  <p class="wp-caption-text">
    Donkey Kong
  </p>
</div>

<div id="attachment_492" style="width: 470px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/zelda.png"><img class="size-full wp-image-492" src="http://personal.network.crazyrobot.net/files/2011/07/zelda.png" alt="" width="460" height="400" /></a>
  
  <p class="wp-caption-text">
    zelda
  </p>
</div>

<div id="attachment_474" style="width: 547px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/karate.png"><img class="size-full wp-image-474" src="http://personal.network.crazyrobot.net/files/2011/07/karate.png" alt="" width="537" height="464" /></a>
  
  <p class="wp-caption-text">
    karate champ
  </p>
</div>

<div id="attachment_477" style="width: 614px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/avengers.png"><img class="size-full wp-image-477" src="http://personal.network.crazyrobot.net/files/2011/07/avengers.png" alt="" width="604" height="291" /></a>
  
  <p class="wp-caption-text">
    Captain America and the Avengers
  </p>
</div>

<div id="attachment_491" style="width: 495px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/mariokart1.png"><img class="size-full wp-image-491" src="http://personal.network.crazyrobot.net/files/2011/07/mariokart1.png" alt="mario kart" width="485" height="223" /></a>
  
  <p class="wp-caption-text">
    mario kart
  </p>
</div>

<div id="attachment_494" style="width: 615px" class="wp-caption aligncenter">
  <a href="http://personal.network.crazyrobot.net/files/2011/07/sf2.png"><img class="size-full wp-image-494" src="http://personal.network.crazyrobot.net/files/2011/07/sf2.png" alt="" width="605" height="1008" /></a>
  
  <p class="wp-caption-text">
    sagat ftw
  </p>
</div>

<p style="text-align: left;">
   Thanks to Javier Moreno, Norman Garcia and Angie Bloor for their comments.
</p>

 [1]: http://research.microsoft.com/en-us/um/people/kopf/pixelart/index.html "depixelizing pixel art"
 [2]: http://www.xach.com/lisp/vecto/ "vecto"