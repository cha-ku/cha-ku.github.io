---
layout: post
title: Learnings from writing Mandelbrot images
---

# Preface

Last year in July or so, I started to seriously learn C++ as it looked like a good choice for a second language. My first language was Python and it served me well for 2 years, but I felt it was a good time to switch to a language which allows to 'peek under the hood' if need arises. Modern C++ has come a long way from the "C with Classes" mindset, incorporating more features which allow high level abstractions while giving the programmer a tight control over the performance. This project was part of the [Advanced C++ Tutorial](https://courses.caveofprogramming.com/p/learn-c-tutorial), although my code deviates from author's.


# Mandelbrot fractal

The Mandelbrot set is a set of values in the complex plane where for every point z, the function f(z) = z^2 + c does not diverge.
What this translates to while implementing in code is somewhat like this:
1. Choose a point in our complex plane say, P (x, y).
2. The test for convergence for a point in Mandelbrot set is to check the absolute value of function f(z), starting with z=0 and c=P. If this absolute value goes greater than 2, the sequence will escape to infinity. The maximum number of iterations per pixel are limited to 256. 
3. For pixels where iterations < 256, assign them a colour value. Else each pixel has black colour.
4. Each pixel is bucketed based on the number of iterations and each bucket is assigned a colour.
5. Now that every pixel has a colour assigned to it, draw the image.

The above steps will give you the most common Mandelbrot shape. Here's mine in monochrome:
![improved mandelbrot](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/improved_mandelbrot.bmp)

Once the value for the pixels has been calculated, we can zoom in on a point by repeating steps from 2 to 5. A number of points and their respective scaling factors can be added using the `addZoom` method for any `FractalCreator` object. The output will be generated iteratively after going through all points in the underlying storage provided by the `ZoomList` object.

My choice of image format for this was [bmp](https://en.wikipedia.org/wiki/BMP_file_format), which as it turns out was a pretty poor choice to write this Mandelbrot data in. The API for updating each pixel takes in x and y coordinates and the data array which contains the image pixel data. This assumes x and y coordinates start at the top left corner of an image whereas those for BMP file start from the bottom left corner which did not bode well once I made changes for the unistride access.
The code for this is available [on github](https://github.com/cha-ku/fractal-demo).

# Gallery

Here is a small gallery of the images obtained 

![original img](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/orig_mb.bmp)
<center><i>Original Mandelbrot</i></center> 

![head and shoulders](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/zoom_mb1.bmp)
<center><i>Intersection between the two heads</i></center>

![seahorse1](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/seahorse1.bmp)
<center><i>The seahorse valley</i></center>

![seahorse2](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/seahorse2.bmp)
<center><i>Zooming in on the seahorse valley</i></center> 

![zoom1](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/zoom_seq1.bmp)
<center><i>Zoom</i></center> 

![zoom2](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/zoom_seq2.bmp)
<center><i>More Zoom</i></center> 

![zoom3](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/zoom_seq3.bmp)
<center><i>Most Zoom</i></center> 

![final one I promise](https://raw.githubusercontent.com/cha-ku/fractal-demo/master/output/test.bmp)
<center><i>Final one I promise</i></center> 
