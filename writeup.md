## Advanced Lane Finding Project

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



## Implementation steps

---

### 1. Camera Calibration and distortion correction


The code for this step is contained in the first code cell of the IPython notebook located in "./Project.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function.

### 2. Pipeline (single images)
Thr pipeline consists of applying color transformation and sobel thresold (see function `Pipeline()` in the IPython notebook).

#### 1. Input

The input of this function is an image which is corrected from distorsion.

#### 2. Processing.

I used a combination of color and gradient thresholds to generate a binary image. A color thresholding is applied on the S-channel of the image (value between 100 and 255 by default). A gradient threshold through x direction is applied on the input image transformed to gray scale.

### 3. Perspective transform (single images)
The code for my perspective transform includes a function called `warp_image(img, src, dst)`in the IPython notebook.  The `warp_image` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:



This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| (565,460)     | (0,0)         | 
| (730,460)     | (1280, 0)     |
| (120,720)     | (0,720)       |
| (1280,720)    | (1280,720)    |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.


### 4. Sliding window algorithm

I aplied the slidng window algorithm in order to fit second degree polynomials for each of the left and right lane (see function `fit_polynomial()` and `find_lane_pixels()` in the IPython notebook).

### 5. Measure curvature radius and vehicle offset to lane center

I did this in the function `measure_curvature_center()` using radius formulas calculation from second degree polynomial.

### 6. Draw lanes on the image.

In this step I created an image with the calculated lanes and the area between them, the I applied the inverse perspective transform, then I combined it with the original image, while adding the calculated radius and offset as a text in the image. (see function `draw_lanes()` in the IPython notebook).


---

## Pipeline (video)

The function `process_image()` performs the whole previous pipeline on a single image.

Here's a [link to my video result](./output_project_video.mp4)

---

### Discussion

#### 1. Issues faced during the implementation

The format of the image (1-channel, 3-channel, RGB, BGR, ...) was a challenge. It needs to be consistent through the whole software pipeline. 

#### 2. Downfalls and further improvements

The previous pipeline could be improved to handle more colors and shadow zones. This involves adding other types of thresholding and color transformations, and further tuning of thresholds.
