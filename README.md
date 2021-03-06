# **Finding Lane Lines on the Road** 
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/btphan95/CarND-LaneLines-P1/master) Click here to launch a live Binder containing the notebook!



When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this simple project, we attempt to mark lane boundaries in several images and videos using an image processing pipeline, implemented in [OpenCV](https://github.com/opencv/opencv), a package that has many useful tools for analyzing images.

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images/solidWhiteCurve.jpg
[image3]: ./output_images/solidWhiteCurve.jpg
[image4]: ./test_images/solidWhiteRight.jpg
[image5]: ./output_images/solidWhiteRight.jpg
[image6]: ./output_images/solidYellowCurve.jpg
[image7]: ./output_images/solidYellowCurve2.jpg
[image8]: ./output_images/solidYellowLeft.jpg
[image9]: ./output_images/whiteCarLaneSwitch.jpg
[image10]: ./test_videos_output/white.gif
[image11]: ./test_videos_output/white_stab.gif
[image12]: ./test_videos_output/yellow.gif
[image13]: ./test_videos_output/yellow_stab.gif
[image14]: ./test_videos_output/challenge_stab.gif

### 1. Image Processing Pipeline

The image processing pipeline consists of 5 steps:
1. Grayscale the image
2. Apply Gaussian blur to the image
3. Apply Canny edge detection
4. Apply a rectangular mask to approximate area of the lane
5. Apply a Hough transform to return the approximation of the left and right lane boundaries

In order to accurately create the lane boundaries, we used the output lines from the Hough transform and:
1. separate the lines from left and right based on the lines' midpoints landing inside a curtain of values.
2. averaging the lines to create one left line and right line
3. Extrapolate the line to mark the entire lane boundaries (note that I chose to crop out the lane boundaries towards the vantage point to prevent obscuring it, for safety considerations).

Here are some examples of the pipeline on images:

![alt text][image3] 
![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]

In videos, the output of the pipeline resulted in slightly shaky and sporadic lines, especially in the yellow lane video.
In order to stabilize this, I created a cache to average out the lines from the last 10 frames. This resulted in more aesthetic lines that can allow the driver to better visualize the lanes while driving, for example.

![alt text][image10]
before stabilization
![alt text][image11]
after stabilization
![alt text][image12]
before stabilization
![alt text][image13]
after stabilization


### 

One potential shortcoming of this image processing pipeline is that it does not perform well on curved roads, roads with obstacles, or images/videos captures under different lighting and environmental conditions than the ones taken in above images/videos.
One primary reason is that the Hough transform was specifically tuned to the images/videos above.
Secondly, the Hough transform may not be suitable for capturing curved roads because those roads would not create lines that the Hough transform could use.
This is best illustrated in a video taken under different lighting on a curved road:

![alt text][image14]

Suggested improvements to the pipeline to improve performance on this video would be to further tune the Hough transform parameters and perhaps use a different algorithm to detect lanes at the end.

