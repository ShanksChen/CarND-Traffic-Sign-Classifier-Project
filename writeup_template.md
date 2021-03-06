# **Traffic Sign Recognition Classifier** 

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./writeup-picture/visualization_dataset.png "visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[image4]: ./new-traffic-signs/30_1.png "New Traffic Sign 1"
[image5]: ./new-traffic-signs/60_3.png "New Traffic Sign 2"
[image6]: ./new-traffic-signs/80_5.png "New Traffic Sign 3"
[image7]: ./new-traffic-signs/bumproad_22.png "New Traffic Sign 4"
[image8]: ./new-traffic-signs/catuion_18.png "New Traffic Sign 5"
[image9]: ./new-traffic-signs/goleft_34.png "New Traffic Sign 6"
[image10]: ./new-traffic-signs/rightofway_11.png "New Traffic Sign 7"
[image11]: ./new-traffic-signs/roadwork_25.png "New Traffic Sign 8"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

Use the dataset from [German Traffic Sign Dataset](http://benchmark.ini.rub.de/?section=gtsrb&subsection=dataset). 
summary statistics of the traffic signs data set:

* The size of training set is 34799.
* The size of the validation set is 4410.
* The size of test set is 12630.
* The shape of a traffic sign image is 32*32 with 3 layers.
* The number of unique classes/labels in the data set is 43.

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set.

![alt text][image1] 

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

First I decided to convert the images to grayscale because the images in the dataset have RGB layers. After grayscale it become one layer. Then I normalized the image data because the image data should be normalized so that the data has mean zero and equal variance. For image data, `(pixel - 128)/ 128` is a quick way to approximately normalize the data and can be used in this project. The result wsa that all pixel value in image data were between -1 and 1.

After nomalized function, I shuffled the image data. Because to avoiding the over fitting.

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model, which is LeNet-5, consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 RGB image   							| 
| Convolution 3x3     	| 1x1 stride, same padding, Outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,     Outputs 14x14x6 				|
| Convolution 3x3	    | 1x1 stride, same padding, Outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,     Outputs 5x5x16 				|
| Flatten	     	 	| Input = 5x5x16. Outputs = 400 				|
| Fully connected		| Input = 400.    Outputs = 120					|
| RELU					|												|
| dropout				|												|
| Fully connected		| Input = 120.    Outputs = 84					|
| RELU					|												|
| dropout				|												|
| Fully connected		| Input = 84.     Outputs = 43					|
|						|												|
 
This architecture have 5 layers:
First layer is a convolution layer, it inclued an convolution, a RELU function and a max pooling function. The convolution used strides 1, padding was valid. The max pooling function used strides 2, padding was valid. The input data size is 32x32x1, after max pooling functiong the output size is 14x14x6.

Second layer is also an convolution layer same as the first layer excepts the data size. The input data size is 14x14x6, output is 5x5x16.

Third layer is a flatten layer, transform the data size from 5x5x16 to 400.

Fourth layer is fully connected layer, it included a matrix multiply, a RELU function and a dropout function. Theinput data size of this layer is 400 and the output is 120.

Fifth layer is also a fully connected layer same as the fourth layer excepts the data size. The input size is 120 and the output is 84.

Sixth layer is a fully connected layer but only had a matrix multiply function. The input size is 84 and the output is 43 which is the same size with the labels.

Why I choose the RELU function. Because compared with Sigmoid, ReLU greatly reduces the computation; on the other hand, when the input signal is strong, the difference between the signals can still be preserved.

And the max pooling function can ensure that the position and rotation of the feature are invariant and reduce overfitting problems.


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an LeNet-5 architecture.

The optimizer is *AdamOptimizer*. It is based on the gradient descent method, but the learning step size of each iteration parameter has a certain range, which does not lead to a large learning step due to a large gradient, and the parameter value is relatively stable.

The batch size is 128, depends on my laptop. This size works well in the class quiz, so I choosen this size for the project.

The unmber of epochs is 60, because I want to get better result. I tried 100, but the accuracy increased very slow after 50. Then I decided to use 60.

The learning rate is 0.001. Bigger learning rate may cause a large gradient descent.

After cross-validation, the implicit node dropout rate is equal to 0.5, which is the best, because the dropout randomly generates the most network structure at 0.5. [1](https://blog.csdn.net/dod_jdi/article/details/78379781) 

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.999.
* validation set accuracy of 0.965.
* test set accuracy of 0.954.

If a well known architecture was chosen:
* What architecture was chosen?
LeNet-5 architecture was chosen.
* Why did you believe it would be relevant to the traffic sign application?
I search the CNN in the [wikipedia](https://en.wikipedia.org/wiki/Convolutional_neural_network). I know this architecture is suitabel for the traffic sign classifier.
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
The accuracy show before is pretty good for this application. When I found some new traffic sign images from web, the accuracy for these images is 100%. So I think it is working well.

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are eight German traffic signs that I found on the web:

![alt text][image4] ![alt text][image5] ![alt text][image6] ![alt text][image7]
![alt text][image8] ![alt text][image9] ![alt text][image10] ![alt text][image11]

The images ware not the requied shape for the input. Then I changed shape to 32x32 by tools in OSX.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image							        |     Prediction	        					| 
|:---------------------:				|:---------------------------------------------:| 
| Speed limit(30km/h)   				| Speed limit(30km/h)    						| 
| Speed limit(60km/h)   				| Speed limit(60km/h)    						| 
| Speed limit(80km/h)   				| Speed limit(60km/h)    						| 
| Bumpy Road							| Children crossing								|
| General caution						| General caution								|
| Turn left ahead 						| Turn left ahead				 				|
| Right-of-way at the next intersection	| Right-of-way at the next intersection			|
| Road work								| Road work										|


The model was able to correctly guess 6 of the 8 traffic signs, which gives an accuracy of 75%. The umber of samples is too small that the accuracy has a big impact.

#### 3. Describe how certain the model is when predicting on each of the eight new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 1th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a general caution (probability of 1), and the image does contain a general caution. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1         			| General caution								| 
| 0     				| Traffic signals								|
| 0						| Keep left										|
| 0	      				| Speed limit (20km/h)			 				|
| 0					    | Pedestrians     								|


For the sixth image, the model think that this is a children crossing (probability of 0.37), and the image does not contain a children crossing, the image contains a bumpy road. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.37         			| Children crossing								| 
| 0.12     				| Bumpy road									|
| 0.12					| Dangerous curve to the right					|
| 0.8	  				| Bicycles crossing								|
| 0.8					| Traffic signals								|


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


