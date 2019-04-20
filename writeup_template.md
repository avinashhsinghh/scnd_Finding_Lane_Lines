# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.

**Step 1:** I converted the images to grayscale.
**Step 2:** I use Gaussian blur to smoothen the image and reduce noise.
**Step 3:** I used Canny edge detection algorithm to find edges of objects.
**Step 4:** I select area where most likely is the road to reduce false positives.
**Step 5:** Here I detect lane lines and highlight them.
* I use Hough Transformation on the found points to find formulas of lines. If enough lines corresponding to found points crosses in Hough space it means that they are one line.

* I filter out the lines with too steep or too flat slope since they are false positives adding too much noise.

* I split set of found lines according to their slope parameter. Those with positive alpha are part of left lane line and those with alpha negative are part of right lane.

* In those groups I compute average slope and the intercept of found functions, giving me approximation of the lane line in current frame.

* Since this approximation, due to shadows, flares etc., can be skewed I average the lane function (slope and intercept) with previous 50 lane functions using Weighted Moving average. I was inspired by Exponential Smoothening but here instead of smoothing the pixels on the image I smooth the functions. The result is more than satisfying.

* Last in this step, by using found function definitions I paint lane highlights.
 
**Step 6:** Exponential Smoothening on painted lane highlights using last 10 frames to reduce flicker.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the road is not in the centre of the video the code needs recalibrating. 

The only drawback is that when I try to run the "challenge.mp4" video, the lane lines are not recognised correctly. There is a lot of flickering and wrong detection in between when the road is curved left or right. This means the current pipeline is not so good for curved roads.

Another shortcoming could be less sunny video, snow or rainy videos.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to deal with sun and shadows we need to use different color channel than RGB. Something like HLS or YCbCr.

Another potential improvement could be to some advanced methods like DNN.

Improvements in the Pipeline are needed so that the curved lane lines are also detected without any error.

