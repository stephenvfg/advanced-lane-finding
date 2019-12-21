# Advanced Lane Finding
## Writeup
---

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

**All referenced source code is from the Jupyter notebook in this repostitory P2.ipynb**
---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for my camera calibration and distortion correction is in the Jupyter notebook cells titled "Supporting Functions" and "Compute Calibration and Undistort Image". The  `calibrate ` function was created for this purpose.

In  `camera_cal ` there are several images of a 9 by 6 chessboard that we will use to calibrate the camera. I defined the  `calibrate ` function that takes as parameters the  `camera_cal/ ` directory path as well as the 9 by 6 dimensions of the chessboard.

```
calibrate('camera_cal/', 9, 6)
```

This function defines an array of 9x6=54 (x,y,z) coordinates. We have an array of object points which will cycle from (0,0,0) to (8,5,0) representing the coordinates of the chessboard corners in the real world. (z is always 0 because the chessboard is flat.) For each chessboard image we will also use  `cv2.findChessboardCorners ` to find the (x,y,z) coordinates of the same corners. These image points will be paired with the earlier object points.

The `calibrate` outputs `objpoints` and `imgpoints` will be used as parameters for `cv2.calibrateCamera()` to calculate the calibration coefficients `mtx` and `dist`. These coefficients are passed into  `cv2.undistort()` along with a test chessboard image to obtain the undistorted chessboard.

# Insert undist_chess

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

The distortion-correction method outlined above was condensed into the `undistort` function in the "Supporting Functions" cell. Applying  this function to our lane images produces the following:

# Insert undist_lane





---
# Example writeup
---

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines # through # in `another_file.py`).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

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
