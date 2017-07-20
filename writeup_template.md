# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/gray.jpg "Grayscale"
[image2]: ./test_images_output/gaussian_blur.jpg "Grayscale"
[image3]: ./test_images_output/canny_edge.jpg "Grayscale"
[image4]: ./test_images_output/roi.jpg "Grayscale"
[image5]: ./test_images_output/output.jpg "Grayscale"



---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale. Second, I used gaussian filter with kernel size 11x11 to make the image look smoother than the original image. Third, the filtered image is passed to Canny edge detection function to find edge lines of the image. Fourth, the edge line image is passed to the region of interest. Fifth, the masked image is passed to hough transform, then generates the output image.

Instead of writing messy code in "draw_lines()"(draw_lines2() in my code) function, I created "averaging()" and "form_line()" functions. "averaging()" function takes all the linear lines of each frames and finds average of the slope and intercept of left and right lines. "form_line()" function calls "averaging()" to generate extrapolated linear lines for both left and right lanes.
Below are averaging() and form_line():
averaging()
'''python
def averaging(lines):

    #This function finds average slope and intercept of lines in a frame.

    left_slope = []
    left_intercept = []
    right_slope = []
    right_intercept = []

    for line in lines:
        if line is not None:
            for x1,y1,x2,y2 in line:
                if x1 != x2:
                    slope = (y2 - y1)/(x2 - x1)
                    b = y1 - slope * x1 # Intercept b from y = slope*x + b
                    if (abs(slope) > 0.5):
                        if slope < 0: #left-side
                            left_slope.append(slope)
                            left_intercept.append(b)
                        else: #right-side
                            right_slope.append(slope)
                            right_intercept.append(b)
        else:
            None

    # Find average of intercept and slope of lines on a frame
    if len(left_slope) > 0:
        avg_left_slope = sum(left_slope)/len(left_slope)
        avg_left_intercept = sum(left_intercept) / len(left_intercept)
    if len(right_slope) > 0:
        avg_right_slope = sum(right_slope)/len(right_slope)
        avg_right_intercept = sum(right_intercept) / len(right_intercept)

    if (avg_left_slope is not None) and (avg_left_intercept is not None) and (avg_right_slope is not None) and (avg_right_intercept is not None):
        return avg_left_slope,avg_left_intercept,avg_right_slope,avg_right_intercept
    else:
        return None
'''
form_line()
'''python
def form_line(img, lines):

    #This function creates a linear line from avg_slope and avg_inetercept.

    if averaging(lines) is None:
        return None
    else:
        #### Extrapolated lines are generated here ####
        avg_left_slope,avg_left_intercept,avg_right_slope,avg_right_intercept = averaging(lines)
        y1 = img.shape[0]
        y2 = y1 * 0.6

        # x-coordinates from equation y = mx + b => x = (y - b)/m
        x1_left = (y1 - avg_left_intercept)/avg_left_slope
        x2_left = (y2 - avg_left_intercept)/avg_left_slope
        x1_right = (y1 - avg_right_intercept)/avg_right_slope
        x2_right = (y2 - avg_right_intercept)/avg_right_slope

        # Convert x1,y1,x2,y2 to int
        y1 = int(y1)
        y2 = int(y2)
        x1_left = int(x1_left)
        x2_left = int(x2_left)
        x1_right = int(x1_right)
        x2_right = int(x2_right)

        return ((x1_left,y1),(x2_left,y2)), ((x1_right,y1),(x2_right,y2)) #left_line, right_line


**Functions used in order**
* Conversion to grayscale
* Gaussian filter
* Canny edge detection
* Region of interest
* Hough transform

Below shows the outputs of the functions listed above:

**Output Images**

![alt text][image1]

![alt text][image2]

![alt text][image3]

![alt text][image4]

![alt text][image5]

**Output Videos**

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/jTXmDoVTzYs/0.jpg)](http://www.youtube.com/watch?v=jTXmDoVTzYs)

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/kx1p7ePWCR0/0.jpg)](http://www.youtube.com/watch?v=kx1p7ePWCR0)

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ...

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
