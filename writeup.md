# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

## Reflection

### 1. Pipeline Description.

#### My pipeline consists of 7 steps:
 1) First, convert images to grayscale (to prepare images for subsequent processing).
 2) Apply Gaussian Smoothing to the images.
 3) Apply Canny Transform on the Gaussian-smoothed gray scale images.
 4) Define a 4-sided polygon mask, and apply it to the Gaussian-smoothed Canny-transformed images,
    to mask out the outside edges of the images, which couldn't possibly contain lane lines.
 5) Create and refine a "lane line image" (to be used by "draw_lines" to overlay each original color image),
    by performing a Hough Transform, to create a lined-image representation in Hough Space. 
 6) Execute the "draw_lines" function (from inside the Hough_lines function),
    to create an image containing lane lines, to be overlayed on each original color image.
 7) Perform the actual overlay of the lane line images, onto each original color image.
 
#### In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
 1) Splitting the hough-transformed lines into "left" and "right" lane lines, based on their slope.
 2) Calculated the "average slope" of each set of lines (left and right sets).
 3) Extrapolated the left and right lane lines to be drawn, by using the "average slope",
    and tactically-selected points, of the left and right lane lines.

#### Following are the required test output images, with lane lines overlayed: 

NOTE: All of the test input images and videos, as well as the test output images and videos from the pipeline,
have been included in the GitHub repository for this project, in the required subdirectories.

solidWhiteRight.jpg:
![solidWhiteRight.jpg](./test_images_output/solidWhiteRight.jpg)
solidYellowLeft.jpg:
![solidYellowleft.jpg](./test_images_output/solidYellowleft.jpg)
solidWhiteCurve.jpg:
![solidWhiteCurve.jpg](./test_images_output/solidWhiteCurve.jpg)
solidYellowCurve.jpg:
![solidYellowCurve.jpg](./test_images_output/solidYellowCurve.jpg)
solidYellowCurve2.jpg:
![solidYellowCurve2.jpg](./test_images_output/solidYellowCurve2.jpg)
whiteCarLaneSwitch.jpg:
![whiteCarLaneSwitch.jpg](./test_images_output/whiteCarLaneSwitch.jpg)

### 2. Identification of a potential shortcoming with the current pipeline implementation

One potential shortcoming happens, at times, when "spurious lines" are detected during the hough transform,
due to "intermittant and spurious artifacts" occurring in the images, generating what I call line "outliers".
These "outliers" cause the "average slope" of all the lines to skew from optimal, and the subsequent
lane lines appearing in the images and videos, to sometimes appear off from the actual lane lines. 


### 3. Suggest possible improvements to your pipeline

Possible improvements, to address the shortcoming describes above, are:
  1) Discard the most "egregious" outliers in the sets of left and right hough lines, used to build the lane lines.
     This would result in a better performing ("higher fidelity" lane line drawing) algorithm.
  2) During video processing, take into account the positions of the lines generated in previous frames, and use
     those positions to "baseline" and "balance" the generated lines, so that they have less variation and provide
     "additional lane line drawing fidelity".
