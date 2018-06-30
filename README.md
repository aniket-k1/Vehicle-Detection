# Vehicle Detection Project

[//]: # (Image References)
[image1]: ./output_images/car_notcar.png
[image2]: ./output_images/carhog.png
[image3]: ./output_images/find1.png
[image4]: ./output_images/find2.png
[image5]: ./output_images/find3.png
[image6]: ./output_images/find4.png
[image7]: ./output_images/find5.png
[image8]: ./output_images/heat1.png
[image9]: ./output_images/heat2.png
[image10]: ./output_images/heat3.png
[image11]: ./output_images/found.png
[image12]: ./output_images/test_images.png

---

### Histogram of Oriented Gradients (HOG)

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]

I tried various combinations of parameters. Changing the color space, orient, pixels per cell, cells per block, and the number of channels from HOG. In trying out different parameters I tried to find parameters that had both high accuracy as well as able to make predictions in a short amount of time. The final parameters were YUV colorspace, 11 orientations, 16 pixels per cell, 2 cells per block, and all channels from the HOG.

* Test Accuracy of SVC =  0.9823
* 0.00188 Seconds to predict 10 labels with SVC

### Sliding Window Search

I used sliding windows of varying different sizes (1x, 1.5x, 2x, 3x). I found that this gave satisfactory results for cars different distances away. I calculate the HOG features for the entire image. These features are then sampled based on the size of the window and then given to the classifier. I have 50% overlap in the X direction and 75% overlap in the Y direction. I want this section of the pipeline to produce more hits for true results so that the heat map described later can be more accurate in filtering out false positives.

Additionally, I store the last 15 detected frames. I combine these frames to the heatmap and set the threshold to `1 + len(det.prev_rects)//2`. Using this technique I'm able to eliminate most of the false positives from being detected.

![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

Here are the results returned by this method. There are multiple detections for the cars in the front and only 1 for the car in the other section of the highway.

![alt text][image7]


Here is a heat map of the above results

![alt text][image8]


Now with a threshold of 1

![alt text][image9]


Here I apply `scipy.ndimage.measurements.label()` to the heat map to identify the different regions

![alt text][image10]


Using the above results I'm able to draw a bounding box on the original image

![alt text][image11]
