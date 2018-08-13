# Udacity_SDC_NanoDegree_Term1_Project4
Advanced Lane Finding Project
---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.



### 1. Camera Calibration

The ChessBoard Images are used to calibrate the camera. OpenCV provides the functions to find the chessboard corners and used them to calculate the camera matrix as well as the distortion coefficients, and in the end undistort the images:

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/chessboard.png)

The Camera matrix and distortion coefficients are used for all the images and the video in this project. The undistortion step is included in the color transform function.


### 2.  Image calibration and then use color transforms, gradients, etc., to create a thresholded binary image.

The final is a combination of binary thresholding the S channel of the HLS space, the B channel (corresponds to blue-yellow) of the LAB space and binary thresholding the result of applying the Sobel operator in the x direction on the original image. The LAB increases significantly the clearness of the yellow lane lines.

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/Image_undistort_and_colorGradient.png)

### 3. Perpective Transform
By using the lines from the two straght line images, a trapezoidal region is chosen to perform the perspective transform. After transform, the trapezoidal becomes a rectangle. An offset is used to remove the inessential part of the original image. The perspective matrix is calculated here for later use of recovery. 

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/perspective.png)


### 4. Lane line fitting
For the tested images, the seraching starts from the two highest peaks of the histogram, then using sliding windows to scan upward. The number of sliding windows (vertical), the width of the window margin (horizontal) as well as the minimum pixels needed to for window recenter are tuned as hyperparameters. Couting the active pixels in the window and compared with the hyperparameter  -- minimum pixels to recenter the window.

After finding all the acvie pixels for lane lines, a 2nd order polynomial function is used to fit the pixels as the lane line for plotting and the calculation of radius.

The approach of sliding windows is used to find the lane line pixels. Then a 2nd order polynomial function is used to fit the lane lines.
![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/polyfitting.png)


### 5. Radius measurement

Then I calculate the radius of the left and right curvature of pixels and transformed them to the real radius in practice. The distance between left and right lane lines are approximately 700 pixels, which is used to convert the pixel value to real values in meter.

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/radius_calculation.png)


#### 6. Wrap to the original images and draw lane lines
In the end, the perspective images are wraped to the original images. The vehicule position is calculated by comparing the center of the lane line and the center of the bottom line of the image. 

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/test_on_images.png)


### Test on video

Here's a [link to my video result](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/project_video_output.mp4)


### Discussion

The first step of image calibration and then use color transforms, gradients, etc., to create a thresholded binary image seems to be the most important step for the whole pipeline. By utilizing the LAB space, the yellow lines become much more clear than without the binary output from B channel. 
In reality, other more complicated or dynamic thresholding process may be used to make the model more robust.
  
