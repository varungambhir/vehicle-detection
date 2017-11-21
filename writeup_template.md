##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/test1.png
[image2]: ./output_images/test2.png
[image3]: ./output_images/test3.png
[image4]: ./output_images/test4.png
[image5]: ./output_images/test5.png
[image6]: ./output_images/test6.png
[video1]: ./project_video_output.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

I created a dataset having vehicle and non-vehicle images and trained my svm classifier to distinguish between a car and noncar image. The input image to the svm was a feature vector where the hog features were extracted having the following parameter used:

1. color_space
2. spatial_size
3. hist_bins
4. orient
5. pix_per_cell
6. cell_per_block
7. hog_channel
8. spatial_feat
9. hist_feat
10. hog_feat

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations where i experimented with different color spaces, pixel per block, cell per block, however i ended up with the following values which gave optimal results:

1. color_space = 'YCrCb'
2. spatial_size = (32, 32)
3. hist_bins = 32
4. orient = 9
5. pix_per_cell = 8
6. cell_per_block = 2
7. hog_channel = 'ALL'
8. spatial_feat = True
9. hist_feat = True
10. hog_feat = True

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using feature vectors where two type of feature vectors were used car and noncar. These feature vectors were first normalized and then were fed into the model to output 1 if it was a car and 0 if it wasnt.

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I used the find_car() function from the lesson to find the car in an image. However to remove multiple boxes around the same car what I did was implemented my algorithm where i took the box coordinates in a vector. Then on these coordinates in a vector i checked whether they overlapped or not which meant that they belong to the same car otherwise the coordinate belonged to  a new car object. Using this new centroid points were found out which were found out through averaging all cases found and then box was drawn around the car object as seen in the video. 

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Results on the test images can be seen below:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

To optimize the performance i chose y range from 400 to 650 as the cars wont be present in the sky so iterating throuhg that part didnt make sense. I used a scale of 1.5 after trying out different values of 1.0 and 2.0 which didnt work. Also chossing one window size worked well in my case and no rescaling was required.

---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_output.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I used the find_car() function from the lesson to find the car in an image. However to remove multiple boxes around the same car what I did was implemented my alogorithm where i took the box coordinates in a vector. Then on these coordinates in a vector i checked whether they overlapped or not which meant that they belong to the same car otherwise the coordinate belonged to  a new car object. Using this new centroid points were found out whioch were found out through averaging all cases found and then box was drawn around the car object as seen in the video. 

### Here are six frames and their corresponding heatmaps:

I didnt use heatmap approach to remove multiple boxes and false positives. I implemented my own algorithm which seems to be working in the video. Hope I am allowed to do that.

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:

I didnt use heatmap approach to remove multiple boxes and false positives. I implemented my own algorithm which seems to be working in the video. Hope I am allowed to do that.

### Here the resulting bounding boxes are drawn onto the last frame in the series:

![alt text][image6]

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I think using svm is not optimal as neural networks might give a better result. Also my algorithm to remove false positives and multiple boxes around same car can be improved to perform better in various conditions.
