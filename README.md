# Advanced Lane Finding

## Introduction

This is the fourth project of the self-driving car engineer nanodegree at Udacity. In this project, several videos which collected by the central camera of a car on different road conditions, were given. We aim to find lanes in those videos.

To this aim, I split the whole process into the following several steps:

1. Compute the camera calibration matrix and distortion coefficients based on a set of chessboard images.
2. Apply a distortion correction to raw images.
3. Use color transforms, gradients, etc., to create a thresholded binary image.
4. Apply a perspective transform to rectify binary image ("birds-eye view").
5. Detect lane pixels and fit to find the lane boundary.
6. Determine the curvature of the lane and vehicle position with respect to center.
7. Warp the detected lane boundaries back onto the original image.
8. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

For the next several sections, I will explain briefly for these steps.

## Camera Calibration
1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt tag](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/output_images/calibration4_undistort.jpg)

## Pipeline (single image)

1. Provide an example of a distortion-corrected image.
![alt tag](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/output_images/signs_vehicles_xygrad_undistort.png)

2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image. Provide an example of a binary image result.

After many experiments, we find that
* a suitable combination of gradient measurements, such as derivative in the x and y direction, magnitude and direction of the gradient is good for detecting of white lane line. But I didn't find a very nice threshold interval or combination so that they can work on finding yellow lane line and combat with shadows.
* When s-channel of HLS representation of an image is used to detect lane lines, one can easily find the yellow one. However, this choice is not good for detecting of white lane line and not very good to eliminate heavy shadows.
* To reliably detect different colors of lane lines under varying degrees of daylight and shadow, we decide to combine the effect of gradients and color selection.
* We have some subtle modification of normal usages. Please refer to concrete functions.

Example:
![alt tag](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/output_images/color_gradient_test.jpg)

3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

Perspective Transform of an image can helps us to obtain the "birds-eye view" of road condition. To compute the perspective transform, we first undistort the image 'test_images/straight_lines2.jpg'. The corresponding result and computer vision functions cv2.getPerspectiveTransform() ares used to calculate the perspective transform and its inverse.

Example:
![alt tag](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/output_images/img_for_pt.jpg)

4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

We split the possible situations into two cases.

* Case One: there is no lane line information for previous frame or for previous several frames. In this case, we first use histogram to find out the left and right bases. Then sliding window search techniques are used to find the lane lines.
* Case Two: lane lines were detected for several previous frames. In this case, we search in a margin around the previous line position.

Example:
![alt tag](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/output_images/lane_detecting_test.jpg)

5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

Definition of curvature: [link](http://www.intmath.com/applications-differentiation/8-radius-curvature.php)

Calculation of Metre per Pixel

We use above picture and the fact (US standard) that the lane is about 3.7 meters wide and the dashed lane lines are 10 feet or 3 meters long to calculate convert units from metre to pixel.

Example:

![alt tag](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/output_images/m2pixel.jpg)

6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

![alt tag](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/output_images/pipeline_output_test.jpg)


## Pipeline (video)
1. Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!)

[Video Link]()

## Discussion

The following points are crucial for a successful algorithm/pipeline.
1. Tuning threshold values and combining threshold. This step is crucial and will affect the further action.
2. Always keep in mind that the lane lines should change continuously. Then it is not hard to find some methods to get a persistent result. For example, use several previous lane lines information, or use the left lane info to rectify correspond right lane, or vise versa.
Allowing more time, the following problem should be solved.
3. It is possible that only one lane appear. 
4. It is possible that we start the car on a tortuous path.

## Algorithms and Pipeline (Personal Notes)

I chose to explain the thought of designing an algorithm just before, or in, or after the corresponding code. Even though, this method lead to a longer file, it should make the process of understanding much more easier. Please click [here](https://github.com/fangchun007/Advanced-Lane-Finding/blob/master/LaneLineProject.ipynb) for my code and detailed explanation.

I chose to create a test video other than testing only on several images to verify an algorithm or pipeline. If you wish to visit those videos, please click [here](https://github.com/fangchun007/Advanced-Lane-Finding/tree/master/output_images). 

## Results (Personal Notes)

All the result were saved in the folder [output_images](https://github.com/fangchun007/Advanced-Lane-Finding/tree/master/output_images). Unfortunately, there is still one mp4 file, named test_combined.mp4, which cannot be uploaded to github because of its large size. 




