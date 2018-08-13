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

The final is a combination of binary thresholding the S channel of the HLS space and binary thresholding the result of applying the Sobel operator in the x direction on the original image. Red channel was also tested but the result becomes worse.

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/Image_undistort_and_colorGradient.png)

### 3. Perpective Transform
By using the lines from the two straght line images, a trapezoidal region is chosen to perform the perspective transform. After transform, the trapezoidal becomes a rectangle. An offset is used to remove the inessential part of the original image. The perspective matrix is calculated here for later use of recovery. 

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/perspective.png)


### 4. Lane line fitting
The approach of sliding windows is used to find the lane line pixels. Then a 2nd order polynomial function is used to fit the lane lines.
![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/polyfitting.png)


### 5. Radius measurement

Then I calculate the radius of the left and right curvature of pixels and transformed them to the real radius in practice.

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/radius_calculation.png)


#### 6. Wrap to the original images and draw lane lines
In the end, the perspective images are wraped to the original images. The vehicule position is calculated by comparing the center of the lane line and the center of the image. 

![alt text](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/test_on_images.png)


### Test on video

Here's a [link to my video result](https://github.com/chaidamu519/Udacity_SDC_NanoDegree_Term1_Project4/blob/master/project_video_output.mp4)


### Discussion

In the test of video, when a complete intensity saturation on the left line is shown on the left image, the pipeline may fail. By using the average of several previous fitting parameters, this problem has been solved partially. I think a more dynamic thresholding process on the original image would make the model more robust.
  
