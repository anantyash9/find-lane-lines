# **Finding Lane Lines on the Road** 
The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

## 1. My Pipeline

The image/frames are passed through these steps
* Convert the image to HSL (Hue, Saturation, Lightness) color space
* Filters out everything that is not a shade white or yellow using thresholds
* Remove noise with Gaussian blur
* Find edges with canny edge detection
* Find lines with Hough lines function 
* Calculate average slope and x,y co-ordinate for left and right lanes.
* Calculate the average position for lane line between the current frame and last 5 frames ( only for video) 
* Draw the lines with a suitable opacity on the original image/frame
<img src='https://github.com/anantyash9/find-lane-lines/blob/master/test_images/solidWhiteCurve.jpg?raw=true' width =400> <img src='https://github.com/anantyash9/find-lane-lines/blob/master/test_images_output/solidWhiteCurve.jpg?raw=true' width =400>
<img src='https://github.com/anantyash9/find-lane-lines/blob/master/test_images/solidWhiteRight.jpg?raw=true' width =400> <img src='https://github.com/anantyash9/find-lane-lines/blob/master/test_images_output/solidWhiteRight.jpg?raw=true' width =400>
<img src='https://github.com/anantyash9/find-lane-lines/blob/master/test_images/solidYellowCurve.jpg?raw=true' width =400> <img src='https://github.com/anantyash9/find-lane-lines/blob/master/test_images_output/solidYellowCurve.jpg?raw=true' width =400>

**Helper Functions** 
I added 3 new helper functions and modified draw_lines() to draw a single line on the left and right lanes.

1. reset_globals() - Resets the global variables. I use if before running each clip/image.
2. hls() - Converts the image to HSL (Hue, Saturation, Lightness) color space. Makes lane line easier to distinguish.
3. lane_pass_filter() - Filters out everything that is not a shade of white or yellow.

**draw_lines()**

I modified draw_lines to calculate the a average slope and x,y position of each lane.

Not all lines are used for calculating the average.
* The lines with absolute slope greater than 2 or less than 1/2 are discarded and are not considered while calculating the average.
* Similarly lines which are entirely on left or right half of the image are considered while calculating the average of their respective lanes. Lines with endpoints in different halves of the image are not included in either lane.

The average slope and average x,y position for each lane is used to calculate the 'C' in Y = MX + C.

The upper and lower limit of the lines is same as region of interest so Y is known for both top and bottom points. Using the Y and calculated C the X is calculated. 

To make the lane lines appear smooth in the video and avoid jitter caused by bad frames the average value of lane position in the last 5 frames is used instead of the one calculated for the current frame.

In case of a bad frame where no lines are found the running average value stored in global variables is used.


## 2. Shortcomings with the pipeline

* The pipeline will not give reliable results when there is a turns or bend visible in the area of interest because it models lanes as straight lines.
* The fact that it uses running average to maintain lane positions means it will be slow to respond to sudden changes in lanes.
* The pipeline will only work for lanes marked with yellow and white.



## 3. Possible improvements to the pipeline

A possible improvement would be to replace lane_pass_filter() with a filter that works (reliably) regardless of the color in which the lane lines are marked.

Another potential improvement could be made to handle changes in lane positions by continuously adjusting how many frames are used to calculate the average value of lanes. If the lane position and slope are changing fast, then the number of frames used to calculate average can be reduced to make it more responsive to these changes and if the lane position is not varying that fast it can be increased to better resist noise or bad frames.
     
