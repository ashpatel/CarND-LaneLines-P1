# **Finding Lane Lines on the Road**

## Writeup


---

**Finding Lane Lines on the Road**

The goals / steps of this project were the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/solidWhiteCurve_F0_V.jpg "HSV(V)"
[image3]: ./test_images_output/solidWhiteCurve_F0_blurred.jpg "blurred"
[image4]: ./test_images_output/solidWhiteCurve_F0_canned.jpg "canned"
[image5]: ./test_images_output/solidWhiteCurve_F0_masked.jpg "masked"
[image6]: ./test_images_output/solidWhiteCurve_F0_hough.jpg "hough"
[image7]: ./test_images_output/solidWhiteCurve_F0_weighted.jpg "weighted"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I ....

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image:

My Pipeline was made up of 6 steps.

1) Converted image to HSV format and used the V Channel. I found this was better than grayscale at being about to pickout the lanes.

![alt text][image2]

2) The image was then blurred

![alt text][image3]

3) The image was then passed thru Canny for edge detection
![alt text][image4]

4 After canny, we mask the image to only focus on the segment of road ahead of us. Removing the horizon and anything on the sides.

![alt text][image5]

5) Then we extract the hough_lines and pass to draw_lines to draw the lines to hi-light the lanes. I modified the draw_lines() function to first seperate the lines into left and right arrays based on positive/negative slopes of the lines. I then use the Numpy.polynomial.polynomial.polyfit() function to fit all the line segments to a polynomial to draw the best line. I found that if I average in the line segments from the previous frame when using polyfit(), then I reduce jitter.

![alt text][image6]

6) Finally we overlay the lane markings with the original image.

![alt text][image7]


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming of my approach is that is relies on hardwiring the percentage of the image above the horizon to mask out, but the horizon is moving due to hills, and that causes extraneous lines to get included, which causes the lines to jitter.

Another shortcoming is that algorithm in draw_lines is assuming that the lanes lines are straight, and curved roads cause problems with this approach.


### 3. Suggest possible improvements to your pipeline

One improvement would be to implement an algorithm that took the hough lines and fitted it to a curve, which would better be able to deal with a curving road.
