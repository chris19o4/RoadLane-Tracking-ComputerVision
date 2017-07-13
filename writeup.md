# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_1_Gray.jpg
[image2]: ./test_images_output/solidWhiteCurve_2_Blurred.jpg
[image3]: ./test_images_output/solidWhiteCurve_3_CannyEdges.jpg
[image4]: ./test_images_output/solidWhiteCurve_4_Masked.jpg
[image5]: ./test_images_output/solidWhiteCurve_6_Result.jpg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps: 

As learned in the first lesson of the course, the first step is to convert the input image from RGB to grayscale.

![alt text][image1]

In the second step, the converted image is blurred using a Gaussian blur with a 5x5 kernel, which was judged to be a good value during lesson 1.

![alt text][image2]

Using the blurred image, edges are detected using the Canny Edge Detection algorithm. Despite the recommended 1:2 or 1:3 ration between the lower and the higher threshold, tests showed that a low threshold of 140 and a higher on of 180 worked pretty well with the provided images.

![alt text][image3]

To avoid working woth irrelevant data, a region of interest in the picture is defined. Any elements outside this region are clipped.
For the region of interest I chose a triangled area, stretching from the bottom left and right corners to the approximate center of the image.

![alt text][image4]

The remaining edges, which are just discrete points, line segments which connect those dots are created via Hough line transformation. These are then projected on the original RGB image.

![alt text][image5]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function. I tried to find the lines on the  left and the right side in order to group them together.
All points on one side, were grouped together and a first degree polynomial was fit to those points using the numpy.polyfit() function.

### 2. Identify potential shortcomings with your current pipeline


Horizontal lines are ignored, which seems to be fine for the highway scenario. In other surroundings they shouldn't be ignored. For example stop lines.

As seen in the result video of the challenge, this approach only works well with straight lanes. Curves are not handled very well though.
Furthermore, different materials and colors of the road impact the edge detection as the contrast between road colors and line colors is too low.

### 3. Suggest possible improvements to your pipeline

In order to be able to detect curved lanes, instead of the Hough line transformation, which just detects straight lines, another algorithm could be applied, which allows interpolation of points distributed in a curved shape.
