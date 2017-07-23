# **Finding Lane Lines on the Road** 


The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteRightMarked.jpg
[image2]: ./test_images_output/solidYellowCurveMarked.jpg
[image3]: ./test_images_output/solidYellowCurve2Marked.jpg
[image4]: ./test_images_output/solidWhiteCurveMarked.jpg
[image5]: ./test_images_output/solidYellowLeftMarked.jpg
[image6]: ./test_images_output/whiteCarLaneSwitchMarked.jpg
[image7]: ./test_images_output/challenge1Marked.jpg

--

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.

 1. Color Selection: Since the input/image provided is in RGB color space, I was able to detect white lines clearly but yellow lanes were
    not very clear in darker places with shadows. So, I tried converting RGB to HSL color space which gave better results. This HSL
    image was finally converted into grayscale image.
 
 2. Region of Interest: Since the assumption is that front facing camera that took the image is mounted in a fixed position on the car, 
    such that the lane lines will always appear in the same general region of the image. I exclude outside the region of our polygon by applying a mask.

 3. Canny edge detection: The idea is to find edges by tracing out the pixels with strongest gradients(slope/ rate of change in pixel value).
    By identitying edges we can detect lanes. I provide the threshold limits to detect strongest edges. If a pixel gradient is higher than the upper threshold,
    the pixel is accepted as an edge. If a pixel gradient value is below the lower threshold, then it is rejected.

 4. Hough Transform: Hough Transform is just the conversion from image space to Hough space i.e line in Hough space can be represented by a single point at 
    position (m, b) in Hough space. I used OpenCV function, HoughLinesP to find lines in the image.

 5. In order to draw a single line on the left and right lanes, I modified the draw_lines() function by calculating slope(m) and intercept(b) for every line. Since
    There are multiple lines, I took an average of slopes and intercepts to get an average line for left and right lane. Since y coordinate is reversed, slope for left lines
    would have negative values and slope for right lane would have positive value.
    
    Initially my lanes were flickering for videos. To remove this noise, I used a feedback methodology where line information from previous frame was used to calculate a weighted average
    slopes and intercepts.



If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]	![alt text][image2]
![alt text][image3]	![alt text][image4]
![alt text][image5]	![alt text][image6]
![alt text][image7]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the road has steep turn or elevation or when weather conditions effects the lightings.


### 3. Suggest possible improvements to your pipeline

I have different vertices and top_y parameter for region of interest for regular and challenge videos as they have different frame size. A possible improvement 
would be to have a generic region of interest which could resize/automate padding around the lanes and readjust itself.

