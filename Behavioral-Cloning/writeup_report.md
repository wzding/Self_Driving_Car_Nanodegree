#**Behavioral Cloning** 

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/original-image.jpg "Original Image"
[image2]: ./examples/cropped-image.jpg "Cropped Image"
[image3]: ./examples/center-image.jpg "Normal Image"
[image4]: ./examples/center-image-flipped.jpg "Flipped Image"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
###Files Submitted & Code Quality

####1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md summarizing the results

####2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

####3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

###Model Architecture and Training Strategy

####1. An appropriate model architecture has been employed

My model is a revised [NVidia Autonomous Car Group](https://devblogs.nvidia.com/parallelforall/deep-learning-self-driving-cars/), which consists of a convolution neural network with 5x5 and 3x3 filter sizes and depths between 24 and 64 (model.py lines 18-24).

The model includes RELU layers to introduce nonlinearity, and the data is normalized in the model using a Keras lambda layer (code line 79). Afterwards, the image is cropped using a Cropping layer (code line 80). Here is an example image of center lane driving and the same image after cropping:

![alt text][image1]
![alt text][image2]

####2. Attempts to reduce overfitting in the model

The model contains a dropout layers in order to reduce overfitting (model.py lines 82). 

The model was trained and validated on different data sets to ensure that the model was not overfitting (code line 98). The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

####3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line 105).

####4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. In addition to the data provided by Udacity, I used the first track and second track data. The simulator provides three different images: center, left and right cameras. Each image was used to train the model. For details about how I created the training data, see the next section. 

###Model Architecture and Training Strategy

####1. Solution Design Approach

The overall strategy for deriving a model architecture was to minimizing the mean square error of validation loss.

My first step was to use a convolution neural network model similar to the NVidia Autonomous Model I thought this model might be appropriate because this network is used for training a real car to drive autonomously.

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was overfitting. 

To combat the overfitting, I modified the model so that 20% of the data is dropped after the 1st fully connected layer.

Then I used the simulator to create more generating data, the following laps were capture:

* One track driving forward of first track.
* One track driving backward of first track.
* One track driving forward of second track. 
* One track driving backward of second track.

The final step was to run the simulator to see how well the car was driving around track one. There were a few spots where the vehicle fell off the track, for examples, it drives too left on the road and hits the bridge. To improve the driving behavior in these cases, I augmented the data set by flipping each images.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

####2. Final Model Architecture

The final model architecture (model.py lines 77-91) consisted of a convolution neural network with the following layers and layer sizes 


####3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Then I repeated this process on track two in order to get more data points.

To augment the data sat, I also flipped images and angles thinking that this would help generating more data. For example, here is an image that has then been flipped:

![alt text][image3]
![alt text][image4]

After the collection process, I had 120114 number of data points.  I finally randomly shuffled the data set and put 20% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was Z as evidenced by comparing the mean square error of training set and validataion set of each epoch. I used an adam optimizer so that manually training the learning rate wasn't necessary.
