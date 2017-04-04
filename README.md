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

## Algorithms and Pipeline

I chose to explain the thought of designing an algorithm just before, or in, or after the corresponding code. Even though, this method lead to a longer file, it should make the process of understanding much more easier. Please click [here]() for my code.

I chose to create a test video other than testing only on several images to verify an algorithm or pipeline. If you wish to visit those videos, please go to either [here]() or [here](). 

## Results

All the result were saved in the folder []().

## Discussion

The following points are crucial for a successful algorithm/pipeline.
1. Tuning threshold values and combining threshold. This step is crucial and will affect the further action.
2. Always keep in mind that the lane lines should change continuously. Then it is not hard to find some methods to get a persistent result. For example, use several previous lane lines information, or use the left lane info to rectify correspond right lane, or vise versa.
Allowing more time, the following problem should be solved.
3. It is possible that only one lane appear. 
4. It is possible that we start the car on a tortuous path.
