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

**Functions used in order**
* Conversion to grayscale
* Gaussian filter
* Canny edge detection
* Region of interest
* Hough transform

Below shows the outputs of the functions listed above:

In order to draw lines on the image, I created "averaging()" and "form_line()" functions. "averaging()" function takes all the linear lines of each frames and finds average of the slope and intercept of left and right lines. "form_line()" function calls "averaging()" to generate extrapolated linear lines for both left and right lanes.

If you'd like to include images to show how the pipeline works, here is how to include an image:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]

Video
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/https://www.youtube.com/watch?v=jTXmDoVTzYs&feature=youtu.be.jpg)](https://www.youtube.com/watch?v=jTXmDoVTzYs&feature=youtu.be)

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ...

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
