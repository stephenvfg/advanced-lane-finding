# Advanced Lane Finding
## Writeup

**All referenced source code is from the Jupyter notebook in this repostitory P2.ipynb**

**This project followed these steps:**
* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

**Link to [Project Rubric](https://review.udacity.com/#!/rubrics/571/view)**

---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for my camera calibration and distortion correction is in the Jupyter notebook cells titled "Supporting Functions" and "Compute Calibration and Undistort Image". The  `calibrate` function was created for this purpose.

In  `camera_cal` there are several images of a 9 by 6 chessboard that we will use to calibrate the camera. I defined the  `calibrate` function that takes as parameters the  `camera_cal/` directory path as well as the 9 by 6 dimensions of the chessboard.

```
calibrate('camera_cal/', 9, 6)
```

This function defines an array of 9x6=54 (x,y,z) coordinates. We have an array of object points which will cycle from (0,0,0) to (8,5,0) representing the coordinates of the chessboard corners in the real world. (z is always 0 because the chessboard is flat.) For each chessboard image we will also use  `cv2.findChessboardCorners` to find the (x,y,z) coordinates of the same corners. These image points will be paired with the earlier object points.

The `calibrate` outputs `objpoints` and `imgpoints` will be used as parameters for `cv2.calibrateCamera()` to calculate the calibration coefficients `mtx` and `dist`. These coefficients are passed into  `cv2.undistort()` along with a test chessboard image to obtain the undistorted chessboard.

# Insert undist_chess

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

The distortion-correction method outlined above was condensed into the `undistort` function in the "Supporting Functions" cell. Applying  this function to our lane images produces the following:

# Insert undist_lane

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used three different methods to produce a combined thresholded binary image. The following functions are defined in the "Supporting Functions" cell.
1. `b_threshold` generates a threshold using B values from the LAB color space to isolate yellow lane lines.
2. `l_threshold` generates a threshold using L values from the HLS color space to isolate white lane lines.
3. `sx_threshold` generates a threshold using Sobel X gradient values to isolate edges.

When these three thresholds are combined we obtain the following result:

# Insert threshold

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The "Apply Perspective Transform" cell in the Jupyter notebook contains my code for transforming images. The key to transforming images is defining a pairs of source and destination points. The following pairs were used in my code:

Point | Source (trapezoid-ish) | Destination (square)
------------ | ------------- | -------------
Top left | (Horizontal midpoint - 50px, 63% down from the top) | (25% from the left, top of the image)
Bottom left | (16% from the left, bottom of the image) | (25% from the left, bottom of the image)
Bottom right | (14% from the right, bottom of the image) | (25% from the right, bottom of the image)
Top right | (Horizontal midpoint + 53px, 63% down from the top) | (25% from the right, top of the image)

Using these defined points as parameters, I called `cv2.getPerspectiveTransform(src, dst)` to obtain the transformation matrix that I needed. Then I passed my image and the transformation matrix into `cv2.warpPerspective` to get my transformed image.

# Insert transform


---
# Example writeup
---


#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
