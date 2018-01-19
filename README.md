# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report

Finding lane lines is an important task for self-driving car. When we drive, we use our eyes to decide where to go. The lane lines on the road act as our constant reference for where to steer the vehicle. It's the same for autonomous vehicles.

Python and OpenCV were used to detect lane lines in this project. I pre-processed the images by grayscalling and Gaussian blurring, then used Canny Edge Detection and Hough Transform line detection to detect the lane lines.

![Finding Lane Lines](/pipeline/solidWhiteRight.gif)

---

## Installation

You can optionally use Anaconda to create an isolated Python environment dedicated to this project. This is recommended as it makes it possible to have a different environment for each project.

```
conda create -n carnd-term1-p1 python=3
source activate carnd-term1-p1
conda install numpy pandas matplotlib moviepy notebook
```

If you don't have **ffmpeg** installed on your computer, you will have to install it for **moviepy** to work. Run the following in your terminal instead.

```
conda install numpy pandas matplotlib notebook
conda install -c conda-forge moviepy ffmpeg
```

## Reflection

### 1. Lane Finding Pipeline

1. **Grayscale**: It's not required to distinguish between yellow and white lane lines in this project. We are only interested in the brightness information in the images.
<img src="/pipeline/gray.jpg" height="270">

2. **Gaussian Blur**: Suppress noise and spurious gradients by averaging before applying edge detection.
<img src="/pipeline/blur.jpg" height="270">

3. **Canny Edge Detection**: Detect edges in the image by computing the gradient.
<img src="/pipeline/canny.jpg" height="270">

4. **Region of Interest**: Assuming the front facing camera that took the image is mounted in a fixed position on the car, such that the lane lines will always appear in the same general region of the image. This step define this region and remove things outside of it.
<img src="/pipeline/roi.jpg" height="270">

5. **Hough Transform**:  Apply Hough Transform to find lane lines within the region of interest.
<img src="/pipeline/hough.jpg" height="270">

6. **Extrapolate Line Segments**: Last step generates multiple line segments. In order to draw a single line on the left and right lanes, line segments were first grouped into left or right lines, determined by their slopes. Then, for each group, numpy.polyfit was used to fit a line among the points.
<img src="/test_images_output/solidWhiteCurve.jpg" height="270">


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when the color of lane lines are not white/yellow, or when there's zebra crossing ahead.

Another shortcoming could be when the car is going up on a steep hill. Because we are using the same region of interest for any images, it will be inaccurate in this case.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be figuring out a way to automatically tune the parameters for Hough Transform and Canny Edge Detection.

Another potential improvement could be to detect the horizon position before defining the region of interest.
