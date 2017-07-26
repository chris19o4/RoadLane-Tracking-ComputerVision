# Project 1: Finding Lane Lines on the Road

![alt text][image5]

When driving, we use our sight to decide where we go. Lane lines on the roads act as our constant reference where to steer our vehicle.
So Naturally one of the first things we want to do in developing a self-driving car is to automatically detect lane lines using computer vision algorithms.


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_1_Gray.jpg
[image2]: ./test_images_output/solidWhiteCurve_2_Blurred.jpg
[image3]: ./test_images_output/solidWhiteCurve_3_CannyEdges.jpg
[image4]: ./test_images_output/solidWhiteCurve_4_Masked.jpg
[image5]: ./test_images_output/solidWhiteCurve_6_Result.jpg

---

### 1. Project Pipeline

My pipeline consists of 5 steps: 

The first step is to convert the input image from RGB to grayscale. This drastically reduces image complexity.

![alt text][image1]

In the second step, the converted image is blurred using a Gaussian blur with a 5x5 kernel.

![alt text][image2]

Using the blurred image, edges are detected using the Canny Edge Detection algorithm. Despite the recommended 1:2 or 1:3 ration between the lower and the higher threshold, tests showed that a low threshold of 140 and a higher on of 180 worked pretty well with the provided images.

![alt text][image3]

To avoid working with irrelevant data, a region of interest in the picture is defined. Any elements outside this region are clipped.
For the region of interest I chose a triangled area, stretching from the bottom left and right corners to the approximate center of the image.

![alt text][image4]

The remaining edges, which are just discrete points, line segments which connect those dots are created via Hough line transformation. These are then projected on the original RGB image.

![alt text][image5]

In order to draw a single line on the left and right lanes, I tried to find the lines on the left and the right side in order to group them together.
All points on one side, were grouped together and a first degree polynomial was fit to those points using the numpy.polyfit() function.

### 2. Identify potential shortcomings with the current pipeline


Horizontal lines are ignored to avoid , which seems to be fine for the given highway scenario. But in other situations horizontal lane markings can not be ignored. For example horizontal stop lines at crossroads.

As seen in the result video of the challenge, this approach only works well with straight lanes. Curves are not handled very well though.
Furthermore, different materials and colors of the road impact the edge detection as the contrast between road colors and line colors is too low.

### 3. Suggest possible improvements to your pipeline

In order to be able to detect curved lanes, instead of the Hough line transformation, which just detects straight lines, another algorithm could be applied, which allows interpolation of points distributed in a curved shape.
