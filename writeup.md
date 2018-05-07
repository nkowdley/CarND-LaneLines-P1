# **Finding Lane Lines on the Road** 

## Writeup

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of a few steps: 

First, I converted the image to grayscale, using the provided function. 

Then, I applied a gaussian blur to the image using the provided function.  Trial and Error showed me that I needed a kernel size of 5.

Next, I used canny edge detection on the gaussian blurred image using the provided function.  Again, I used trial and error to determine that I needed a low threshold of 75 and a high threshold of 150.

Then, I applied a mask to limit the area that I wished to look at.  Here, I took advantage of the images fixed resolution, and hardcoded my numbers.  I determined these numbers by using the graph axes provided by the image output.

Finally, I did a Hough Transform on the image using the provided function.  Using trial and error, I determined the inputs to this function.  The output of this function was the image with the lane lines drawn onto it.

The Hough Transform function used a function called draw lines to actually draw the lane lines.  This function took in a number of lines and drew each one individually.  I modified this function in order to draw a single continuous line for each lane line. The first thing I noticed was that there were 2 lane lines that I would need to draw for each picture.  Since this meant that I would be duplicating code for each line, I decided to create a class named Line which would hold all the values for each line.  This also meant that I could hide away a lot of the code and make easier incremental changes to it.  

The way I decided which line belonged to which lane line(each line either belonged to the left or right lane), was slope.  The left lane line would have a negative slope, and the right lane line would have a positive slope.  From there, I would calculate the average slope, and y intercept of each lane line, and keep track of the minimum and maximum x coordinates of each line.  Then, I would determine the the corresponding y coordinates of the minimum and maximum x values using the line equation: y = mx + b, and pass these to opencv's line method.

This resulted in the following output: 
[//]: # (Image References)
[FinalOutput]: ./test_images_output/solidYellowCurve.jpg "Final Output"


### 2. Identify potential shortcomings with your current pipeline and 3. Suggest possible improvements to your pipeline

There are a few shortcomings to my current pipeline.

1) Image resolution.  My code is highly dependent on the resolution of the images.  I determined quite quickly that each image was the same resolution, and each video was too.  My image mask, which limits my region of interest uses hardcoded values for this.  If I were to fix this, I would use a more dyanmic image mask.

2) Threshold.  I feel that my threshold was quite high, as I missed some of the smaller lane lines. I felt that I needed to be more strict on my lane lines to prevent possible outliers.    I would probably relax my thresholds a bit in order to get a better output.

3) Non-linear Lane Lines.  This whole project is based on the idea of linear lane lines.  While in some cases this may work, this will not work in all cases. If I could improve this pipeline, I would figure out how to fit all the coordinates to a polynomial function, and draw the output of that function.
