# **Finding Lane Lines on the Road** 

---

### Reflection

### 1. Description of the pipeline

My pipeline consisted of 5 steps. 

1. First, I convert the images to grayscale;
2. Then I apply a Gaussian filter on the resulting grayscale images to remove noise and spurious gradients;
3. I apply the Canny edge detector on the previous resulting image. 
4. The resulting edges are masked in accordance to a user defined region of interest on the images. 
5. Finally I apply the Hough filter on the masked images to get the interesting line coordinates. The lines are drawn on the masked edges images before being combined with the original images.


In order to draw a single line on the left and right lanes, I modify the draw_lines() function by:

+ sorting the slopes of the resulting lines segments from the Hough transform. The goal here is to identify the line segments belonging to the left or to the right lanes,
+ removing the outliers line segments by setting a threshold for the slopes,
+ calculating the average slope and intercept for the right and left lines 
+ extrapolating to the top and bottom of the right and left lanes using a first order polynomial represented by the terms previously calculated (average slope and intercept).

To avoid jittery lines in the videos I buffer the polynomial terms and apply a moving average to estimate the polynomial terms of the lines displayed on each frame. The moving average is applied to at most the recent fifteen videos frames and starts at the third video frame.


### 2. Potential shortcomings with the implemented pipeline

Under cloudy, sunny, rainy weather conditions it could be difficult to detect the lanes edges. The edge detection in the night or on poorly maintained streets could also be difficult to achieve.

Another shortcoming could be approximating curvy lanes since the corresponding coordinates are non linear. Therefore can not fit a first order polynomial.


### 3. Possible improvements to the implemented pipeline

A possible improvement would be to convert the images using HSV colour space and dilate/erode Gaussian blur before the edge detection.[^fn1]

Another potential improvement for lane detection could be to use a lane detection with RANSAC and KALMAN filter as described in this [paper](https://pdfs.semanticscholar.org/8469/c6bc70c7314a69c0736ac59b84baa402d088.pdf)


[^fn1]: Computer Vision and Graphics, International Conference, ICCVG 2010, Leonard Bolc
