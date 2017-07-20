# **Finding Lane Lines on the Road** 


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_output.jpg "Grayscale" 

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

    1. Convert the image to greyscale.
    2. Gaussian blur the image with kernelsize = 3 to reduce the noise and smooth out the image.
    3. Use Canny edge detection to find out all the edges with low threshold of 100 and high 
       threshold of 200.
    4. Crop the edge detection result to the certain region of interest.
    5. Use hough transform to find out the lines.
    6. Use draw_lines() function to manipulate the hough transform result so that only lane lines are drawn.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by the following steps:

    1. By tuning the parameters in the canny edge detection and hough transform, I managed to get a cleaner
       result. The remaining task is to separate the left lane and right lane, filter out the noise and draw
       the lane lines.
    2. Given the result of the hough transform is a set of points (x1, y1, x2, y2) which represents the two
       ending points of line, extract the slope and bias of every line. 
    3. Since the two lane lines are supposed to have distinct slope. We can separate all the lines by their
       slope. Here the split criteria is set to slope > 0.
    4. However, the noise coming from hough transform result will also be splited into these two groups. In
       order to reject the noise, we will take the mean and standard deviation of two group and reject the 
       outliers which is more than one std away from the mean.
    5. After rejecting the outliers, mean slope and bias will be computed for left and right lanes.
    6. Draw the two lane lines and composite them with the original images. See the followin image.

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


Based on the video test result, the yellow lane line detection looks quite jumpy. 

Also, the division computation I used to extract the lane line bias leads NaN while calculating the
challenge problems.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use the pre-built libraries (such as sklearn) to fit the lane line, which
coule have advanced features of automated rejecting outliers and better fit.

Another potential improvement could be to improve the lane line drawing method so that the challenge problems
could be solved.
