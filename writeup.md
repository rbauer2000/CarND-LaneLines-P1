 **Finding Lane Lines on the Road** 

[before_mod]: ./test_images_output/image_proc0.jpg "Before Modification"
[gray]: ./test_images_output/grayscale0.jpg "Grayscale"
[blur_gray]: ./test_images_output/blur_gray0.jpg "Blur Gray"
[canny_edges]: ./test_images_output/edges0.jpg "Canny Edges"
[marked_edges]: ./test_images_output/marked_edges0.jpg "Marked Edges"
[lines_image]: ./test_images_output/lines_image0.jpg "Lines Images"
[single_lines]: ./test_images_output/image_single_line0.jpg "Final Single lines"

**The goals / steps of this project are the following:**

* Make a pipeline that finds lane lines on the road

* Reflect on your work in a written report

### Reflection

### 1. Description of pipeline and how I modified the draw_lines() function.

**My pipeline consisted of 5 steps.**
(The images below were produced after the modification to the draw_lines() function, so a single line is drawn on each side)

1. Convert the images to grayscale.

![gray scale][gray]

2. Apply Gaussian Noise Kernel to smooth the image in order to remove the noise.

![blur gray][blur_gray]

3. Apply Canny Transform which finds intensity gradients of the image.

![canny edges][canny_edges]

4. Mark the region of interest.  Assume camera mounted on vehicle and so lanes will appear in same region of image.

![marked edges][marked_edges]

5. Apply Hough to detect lines.  This function calls draw_lines() function to draw the lanes.

![lines image][lines_image]

And finally the final output, lines(lanes) drawn on original image.  This is result after the modification to draw single lines.  See below for result before modification

![final result][single_lines]

**Modification of draw_lines() function to make a single line**

Here is the result before I modified draw_lines() function. (I set color to blue when I was working on this before modification.  I'm color blind and so don't see the red against the black asphalt very well.  I ended up going back to red and making bold lines)

![lines before modification][before_mod]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
1. Separate the left and right lanes according to the slope: negative slope, left and positive slop, right.
2. For each side, find the average slope of the lines.
3. For each side, find the point highest (smallest y value) of all points on the lines.
4. For each side, using the average slope and highest point, find point along bottom of image (y = 960).  Then, draw single line from this point along bottom edge to highest point.  

(There might be better ways, but I like this simplicity.  E.g. it seems the lines detected run parallel as the left and right side of the paint have lines detected.  So it may be more accurate to find the a point on a line in between these lines.  But that would introduce more complexity and the this point would differ from my point very little.)


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when going around a curve and so the lanes are not straight lines. 

Another shortcoming could be changing conditions, brightness of light, dry vs wet roads, daytime vs nighttime, etc.

And another shortcoming would be at times of construction, lines may not be clearly marked or if the lines haven't been painted in a long time and so are worn.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to modify hough to detect more than just straight lines and instead curved lines.

Another potential improvement could be to find a way to automatically adjust the parameters (kernel size and Hough transform parameters) for different conditions.

Also, I think using information for other sensors and previous mapping of the area to help.
