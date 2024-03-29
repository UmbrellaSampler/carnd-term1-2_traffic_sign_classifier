# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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

[image1]: ./writeup_images/training_set_histogram.png "Training Set Histogram"
[image2]: ./writeup_images/validation_set_histogram.png "Valiation Set Histogram"
[image3]: ./writeup_images/test_set_histogram.png "Test Set Histogram"
[image4]: ./writeup_images/training_set_examples.png "Training Set Examples"
[image5]: ./examples/grayscale.jpg "Grayscale"
[image6]: ./traffic-signs-data/Yield.PNG "Stop Sign 1"
[image7]: ./traffic-signs-data/Stop.PNG "Traffic Sign 2"
[image8]: ./traffic-signs-data/TurnRight.PNG "Traffic Sign 3"
[image9]: ./traffic-signs-data/KeepLeft.PNG "Traffic Sign 4"
[image10]: ./traffic-signs-data/PriorityRoad.PNG "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart for each data set showing how the data and one images of each class label.

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale for three reasons:

* The training set consists of both grayscale and colored images. The training set gets harmonized by doing so.
* The applied model architecture was originaly designed for classifying grayscale images.
* Processing is faster since the amount of input data is less.

Here is an example of a traffic sign image before and after grayscaling.

![alt text][image5]

As a last step, I normalized the image data because processing of normalized data is numerically more stable, especially in the context of global optimization. 

I decided not to generate additional data because the training works satisfactory with grayscaling and normalization only.


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 RGB image   							| 
| Convolution 3x3     	| 1x1 stride, same padding, outputs 32x32x64 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 16x16x64 				|
| Convolution 3x3	    | etc.      									|
| Fully connected		| etc.        									|
| Softmax				| etc.        									|
|						|												|
|						|												|

| Layer         		|     Description	        					|
|:---------------------:|:---------------------------------------------:|
| Input         		| 32x32x1 Grayscale image   					|
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x10 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x10					|
| Convolution 5x5	    | 1x1 stride, valid padding, outputs 10x10x32   |
| RELU       			| 												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x32			        |
| Flatten 				| outputs 800 									|
| Fully connected		| outputs 150                   				|
| RELU				    |      											|
| Dropout				|												|
| Fully connected		| outputs 100      								|
| RELU					|      											|
| Dropout				|												|
| Fully connected		| outputs 43 									|
 


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used the Adams optimizer. The training was executed in batches of size 128 and 35 epochs with a learning rate of 0.0009. The keep probablity for the dropout layers was set to 0.5. 

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of  0.999
* validation set accuracy of 0.971
* test set accuracy of 0.955

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?

The first architecture was LeNet that has been introduced in the lectures. It is known for working well in classifying images.

* What were some problems with the initial architecture?

The accuracy of the validation set was just insufficient.

* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.

In order to avoid overfitting, I added dropout layers to the fully connected layers. This led to a high and more comparable validation and training accuracy and thus to more robust model.  

* Which parameters were tuned? How were they adjusted and why?

I experimented with the keep probability and it turned out that a value of 0.5 worked best for the training. I also tuned the output sizes of the convolutional and the fully connected layers. And increased improved the validation accuracy significantly.

* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

Convolutional layers are great at extracting image features. The main reason for it that convolution in general preseves spatial relationalship between image pixels.
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image6] ![alt text][image7] ![alt text][image8] 
![alt text][image9] ![alt text][image10]

They should be all equally good predictable because the image features of the signs are not disturbed by environmental structures.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:|
| Yield					| Yield											|
| Stop Sign      		| Stop    										| 
| Turn Right Ahead    	| Turn Right Ahead 								|
| Keep Left	      		| Keep Left						 				|
| Priority				| Priority      								|


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. This compares favorably to the accuracy on the test set of 0.955

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

For all predictions and in the limits of numerical accuracy the model predicts with a certainty of 100%. This might due to fact that these images were not taken by a camera but artificially created.

For the first image, the model is 100% sure that this is a yield sign (probability of 1.0). The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Yield   										| 
| 0.0     				| Speed limit (20km/h) 							|
| 0.0					| Speed limit (50km/h)							|
| 0.0	      			| Speed limit (60km/h)		 					|
| 0.0				    | Speed limit (70km/h)     								|

For the second image, the model is 100% sure that this is a stop sign (probability of 1.0). The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Stop   										| 
| 8.22e-12     		| Wild animals crossing 							|
| 2.95e-15			| No entry							|
| 6.44e-17	      	| Keep right		 					|
| 1.21e-18			| Turn left ahead     								|

For the third image, the model is 100% sure that this is a turn right ahead sign (probability of 1.0). The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Turn Right Ahead   										| 
| 2.15e-18     				| Keep left 							|
| 4.45e-28					| Stop 							|
| 1.06e-28	      			| Ahead only		 					|
| 1.00e-30				    | Go straight or left     								|

For the first image, the model is 100% sure that this is a keep left sign (probability of 1.0). The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Keep Left   										| 
| 5.97e-21     				| Yield 							|
| 2.64e-21					| Go straight or left							|
| 2.36e-23	      			| Turn Right Ahead		 					|
| 6.09e-29				    | Dangerous curve to the left     						|

For the first image, the model is 100% sure that this is a priority sign (probability of 1.0). The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Priority   										| 
| 3.40e-10     				| Roundabout mandatory 							|
| 1.36e-12					| Yield							|
| 1.62e-15	      			| Right-of-way at the next intersection			|
| 1.28e-15				    | Speed limit (100km/h)     						|
