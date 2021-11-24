# Classifying Images Based on Species 
![](https://images.unsplash.com/photo-1551969014-7d2c4cddf0b6?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max)
---
### Description
I found inspiration for my project after discovering Microsoft's AI for Earth [website](https://www.microsoft.com/en-us/ai/ai-for-earth). At first I wanted to use camera trap photos for classifying different types of species but the data sets from [Lila](https://lila.science/) were too large to run on my local computer. Intead I found a subset of a larger data set from iNaturalist that I was able to run on my local computer. The iNaturalist images I used from this smaller data set comes from the years 2017, which I used as my trainind data, and 2018, which I used as my testing data. Each data set had over 40 classes but because I had limited computional resources I decided to use only four classes for the training and testing set; each of these four classes were the same across both training and testing. 

---
### Data 
There are a total of 7,655 images for the training set and 5,396 images for the training set. Each dataset contained species from the supercategories Mammalia and Aves. My computional resources were limited and I wasn't able to process the entire training or testing set so I had to create a smaller subset of images from the larger set. Below is a description of the data sets. 

| Type                 | Number of Images | Number of Classes |
|----------------------|------------------|-------------------|
| train data           | 7,655            | 42                |
| test data            | 5,396            | 61                |
| subset of train data | 519              | 4                 |
| subset of test data  | 349              | 4                 |

---
### Proccesing the Images 
Before feeding these images through the model, I preformed image augmentation using the `ImageDataGenerator` from keras. Augmenting the image data is a way of artificially expanding my training set data. It also gives the model a different perspective of these animals which can reduce the variation in the model.

There are different types of argumentation techniques but the augmentation techniques I used were: rescale, rotate, wide shift, height shift, shear (which stretches the images), zoom, horizontal flip, brightness, and fill mode. I set fill mode to reflect. 

### Model
For this project I implemented transfer learning -- a method where a model that is developed for a certain task is reused at the starting point for a model on a second task. In general the approach is to select a pre-trained model, reuse the model (keeping lower layers frozen and adding some new layers), and then fine-tuning the model.

The model I chose to use for classifying the animals in my image data set is the EfficientNet B0 model. The weights for the EfficientNet B0 model are trained on the imagenet dataset which contains more than 14 million images and 1000 classes. When I trained the top layer, I didn't include top weights, freeze trainable layers, add dropout rate, batch normalization, and set epochs to be 20. I kept the learning rate large to avoid overfitting the data. To fine-tune the lower layers, I unfroze the lower layers, increased the epochs to 100, and kept the other parameters the same. I also decreased the learning rate.

---
### Evaluation
For my loss function I used sparse categorical cross entropy and for my metric I used accuracy. My classes are number values instead of being string values. An example is the Swainson’s Thrush species class is labeled 1. 

When I evaluate on the entire testing set and the entire training set, I received a 95% accuracy and 96% accuracy respectively. Below is a table of the number of correctly classified images and incorrectly classified images by class. 

| Class | Number of Images | Number of Images Correctly Classified  | Number of Images Incorrectly Classified |
|-------|------------------|----------------------------------------|-----------------------------------------|
| 0     | 86               | 86                                     | 0                                       |
| 1     | 92               | 90                                     | 2                                       |
| 2     | 86               | 85                                     | 1                                       |
| 3     | 85               | 72                                     | 13                                      |

#### Conclusion
Although I did have some computional limitations, I believe it's possible to scale up this model using the full training and testing set. Before scaling up this model it's important to think about class balance as many of these classes do not contain the same amount of images. Another thing to think about before scaling up is the types of images present in the dataset -- there are some images of paw prints that should be removed as they don't contain any animals in the image. 
