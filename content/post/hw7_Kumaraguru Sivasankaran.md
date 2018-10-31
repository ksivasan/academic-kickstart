+++
# Date this page was created.
date = "2018-10-30"

# Project title.
title = "Using Local Binary Pattern for Feature Extraction in an Image classification problem"

# Project summary to display on homepage.
summary = "An interesting way to create features from images that are gray-scale invariant and rotational invariant for an image classification problem using KNN algorithm"

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "lbp.jpg"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["computer-vision", "machine-learning"]

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = true

# Optional featured image (relative to `static/img/` folder).
[header]
image = "lbp.jpg"
caption = "image courtesy: https://liris.cnrs.fr/Documents/Liris-5004.pdf"

+++

### Local Binary Pattern (LBP) method for feature extraction

The overall aim of this homework is to develop a feature extraction method based on Local Binary Pattern and use it as input to a machine learning algorithm like K-Nearest Neighbour for image classification task.  

This part explains how to extract features from a given image. Through LBP method, we want to ensure feature extraction from an image class that is rotational invariance and color invariance. We start with converting the input RGB image into grayscale image. The crux of LBP involves finding the set of neighbours around a given pixel at equal distance and equally spaced. This is simply the points on a circle of radius R at equidistant angles. We can choose as many as neighbours we would like to have, say P. The coordinates of the neighbours are thus given by $$(\Delta u, \Delta v ) = ( R*\text{cos}\frac{2\pi p}{P}, R*\text{sin} \frac{2\pi p}{P})$$ for all p=0,1,2..P-1.

Given the location of neighbours for the given pixel, we use bilinear interpolation for getting the value of pixel at the coordinates that are not integer values. Since we have a circular neighbourhood, we have atleast 4 neighbours whose values can be obtained directly and the rest require bilinear interpolation.

A simple implementation requires us to calculate the the delta values $(\Delta u, \Delta v)$ using above formula, then add to the coordinates of the current pixel say $(k,l)$ to get $(k+\Delta u,l+\Delta v)$. We can then find the base coordinates using floor function (rounding to next lowest integer), say $(k_{base}, l_{base})$. Then, calculate the actual $(\Delta_k, \Delta_l) = (k-k_{base}, l-l_{base})$ which is supposed to be both positive. Thus we have the point to be interpolated in a square whose coordinates are $(k_{base}+\Delta_k, l_{base} + \Delta_l)$. 

Now, the bilinear interpolation gives,
$$pixel(k_{base}+\Delta k, l_{base} + \Delta l) = (1-\Delta k)(1-\Delta l)*pixel(k_{base}, l_{base}) + (1-\Delta k)\Delta l*pixel(k_{base}, l_{base}+1) + 
(\Delta k)(1-\Delta l)*pixel(k_{base}+1, l_{base}) +  \Delta k\Delta l*pixel(k_{base}+1, l_{base}+1) $$

Thus, we get a list of pixel values of all our neighbours. Now, threshold these values at 1 if pixel value in list is greater or equal to the pixel value at present coordinate. We end up with a pattern of zeros and ones with length equal to number of neighbours P. 

Here, we apply the runs algorithm to get the minimum of all possible values when this pattern is rotated (to ensure rotational invariance). By rotating the pattern for P times in circular fashion, we end up getting all patterns possible and choose the lowest one as minimum vector pattern. I converted each pattern into decimal value and chose the lowest among them all. The minimum binary pattern is saved (without padding zeros in the beginning of binary number)

After this step, the obtained binary pattern has to be encoded with values (0, 1, ..., P+2) following the criteria:
* If all the digits in the pattern are 0's, represent it by 0
* If all the digits in the pattern are 1's, represent it by the number of ones in the pattern.
* For all others, represent it by P+1.

At this point, we have found the encoding for a given pixel coordinate in an image. We can run this through all of the image pixels and get the encoded values. The count of the encoded values are stored in a histogram which gives a feature vector for the given image. By doing this for all images in the training set, we get a library of feature vectors and corresponding labels.

#### Nearest neighbour classifier

In order to predict a class for a test image, we convert the test image into a feature vector containing the histogram information of the encoded values as discussed in above step. By using an Euclidean norm, we can find the distance between this feature and all features available in training set examples. Then, choose the k-nearest training samples and get the corresponding labels. Assign the label which appears maximum number of times amongst all k-nearest neighbours to the test image.

Euclidean norm for x= training_feature - test_feature is given by:
$$||x||_2=(x_1^2 + x_2^2 +\dots+ x_n^2)^{0.5}$$

#### KL Divergence

Out of curiosity, I used another measure instead of euclidean norm to decide the neighbours. In this metric, the resulting histogram is normalized and hence, we get a probability distribution with support 0 to P+2. These probability distributions can be compared using KL Divergence which is just $$D_{KL}(p||q) = \sum_{x_i} p(x_i) [log(p(x_i) - log(q(x_i))]$$ KL divergence basically measures how close is a probability distribution to another.

#### LBP Histogram 

Histogram vector can be read as no. of encoded values between 0 to 8. 
#### Beach

[22304, 30319, 19370, 38730, 52493, 63513, 24287, 45732, 76543, 50447]

[27860, 51903, 17611, 34337, 55519, 92450, 26463, 78883, 173214, 72496]


#### Building

[3087, 4823, 2309, 4849, 8568, 6346, 3038, 4799, 4756, 6977]

[3519, 3952, 945, 3695, 5628, 6651, 2078, 6122, 8898, 7925]


#### Car

[3260, 4517, 2041, 4300, 9423, 6825, 3077, 4465, 4904, 6782]

[2910, 5198, 2169, 3751, 7432, 5803, 2793, 5459, 6596, 7575]


#### Mountain

[3666, 4540, 3283, 4983, 7329, 5310, 3785, 4548, 4706, 7350]

[2761, 3739, 2192, 4327, 8074, 7955, 3453, 4538, 5751, 6623]


#### Tree

[4887, 4968, 3412, 4160, 4076, 3674, 3055, 4902, 6817, 9462

[4996, 5439, 2820, 3947, 4584, 3707, 2687, 5240, 6399, 9775]

#### Performance measures


```python
print(df_confusion)
```

    Predicted  0  1  2  3  4  All
    Actual                       
    0          5  0  0  0  0    5
    1          0  3  0  2  0    5
    2          0  2  3  0  0    5
    3          0  3  0  2  0    5
    4          0  1  0  1  3    5
    All        5  9  3  5  3   25
    


```python
print(df_confusion_kl)
```

    Predicted_kl    0  1  2  3  4  All
    Actual_kl                         
    0               5  0  0  0  0    5
    1               2  1  0  1  1    5
    2               1  1  3  0  0    5
    3               2  0  0  3  0    5
    4               0  1  0  1  3    5
    All            10  3  3  5  4   25
    

#### Observation on the performance

As we see the accuracy of the K-NN classifier is around 60% when using Euclidean norm as metric and 5 nearest neighbours were considered. The accuracy of the K-NN classifier increased upto 72% when 10 nearest neighbours were considered. The KL divergence metric gave similar results but did poorly when number of neighbours considered were increased. We can infer few more things from the LBP histogram. The class Beach stands out of all and hence is predicted pretty well. The mountain and building class are confused with each other and has low accuracy. The car class is also predicted wrongly as building. Having said that, the histograms of mountain, car, building has similar structure. This questions if the number of neighbours and radius of interest are appropriate in these cases (unlike beach). Thus to tackle this issue, we might considering changing the value of R and P. 

During this task, it was observed that the time taken to get the LBP histogram was longer and ways of optimizing the code has to be explored in future. The code was slow due to presumably two factors: two for loops and individual access of pixel values. Since this code was written focused on accepting any value of P and R, we can convert in the optimized version (of known P and R) with precalculated weights for bilinear interpolation and having a vectorized approach of multiplication (similar to applying a filter/ convolution). This could save running time and allow us to experiment more. 

In another instance, we can consider creating feature vectors by concatenating results from two (or more) different P and R values. Also, a better ML algorithm could improve efficiency as KNN does not learn any structure in training step and hence, necessarily cannot generalize on slightly different test data distribution. 

#### Source code


```python
# -*- coding: utf-8 -*-
"""
Created on Mon Oct 29 10:57:56 2018
@author: Kumaraguru
"""
import numpy as np
import cv2
import os
import math
training_folder = r"C:\Users\Kumaraguru\OneDrive - purdue.edu\Graduate studies\ECE661\Homework 7\training"
testing_folder = r"C:\Users\Kumaraguru\OneDrive - purdue.edu\Graduate studies\ECE661\\Homework 7\testing"

def load_images_from_folder(folder):
    images = []
    for filename in os.listdir(folder):
        img = cv2.imread(os.path.join(folder,filename))
        if img is not None:
            images.append(img)
    return images

# read training dataset
training_images = []
for classname in os.listdir(training_folder):
    subfolder = os.path.join(training_folder, classname)
    training_images.append(load_images_from_folder(subfolder))
# read test dataset
test_images = load_images_from_folder(testing_folder)

# Function to Convert binary minVectors to encoding: 0 to P+1
def Encoding(binary,P):
    bin_List = list(binary)
    binInt_List = [int(c) for c in bin_List]
    summation = sum(binInt_List)
    if summation == 0:
        encode = 0
    elif summation == len(bin_List):
        encode = summation
    else:
        encode = P+1
    return encode

# do the rotations to get the minimum value of the pattern
# remember this is for rotational invariance       
def patternRuns(pattern):
    maxValue= 10e10
    for i in range(len(pattern)):
        sampleValue = int(''.join(map(str,pattern)),2)
        if sampleValue < maxValue:
            maxValue = sampleValue
        a = pattern.pop()
        pattern = [a] + pattern   
    return bin(maxValue)[2:]

def LocalBinaryPattern(RGBImage):
    # Uncomment if using Gray image 
    # Gray_Image = RGBImage
    # comment if using Gray image
    Gray_Image = cv2.cvtColor(RGBImage, cv2.COLOR_BGR2GRAY)
    # set no. of neighbours P and Radius of interest R
    R = 1
    P= 8
    histogram = {samplerange:0 for samplerange in range(0, P+2)}
    rowMax, colMax = np.shape(Gray_Image)
    
    for i in range(R, rowMax-R):
        for j in range(R, colMax-R):
            pattern = []
            pixel = []
            pixel.append(float(Gray_Image[i][j]))
            # extract the gray-levels of neighbours in some specific order
            # clockwise or counter clockwise
            for p in range(0,P):
                #print(p)
                del_k, del_l = R*math.cos(2*math.pi*p/P),R*math.sin(2*math.pi*p/P)   
                if abs(del_k) < 0.001: del_k = 0.0
                if abs(del_l) < 0.001: del_l = 0.0
                k, l = i + del_k, j+del_l
                k_base, l_base = int(k) , int(l)
                delta_k = k-k_base
                delta_l = l-l_base
                if delta_k < 1e-4 and delta_l < 1e-4:
                    pixel_val = float(Gray_Image[k_base,l_base])
                else:
                    pixel_val = (1-delta_k)*(1-delta_l)*Gray_Image[k_base, l_base] + \
                                delta_k*delta_l*Gray_Image[k_base+1, l_base+1] + \
                                (1-delta_k)*delta_l*Gray_Image[k_base, l_base+1] + \
                                delta_k*(1-delta_l)*Gray_Image[k_base+1, l_base]
                pixel.append(pixel_val)
                # convert it into binary
                if pixel_val >= float(Gray_Image[i,j]):
                    pattern.append(1)
                else:
                    pattern.append(0)
            #print("pixel at ({},{})".format(i,j))
            #print(pixel)
            #print(pattern)
            minRuns = patternRuns(pattern)
            encode = Encoding(minRuns, P)         
            # get histogram of all points in image
            histogram[encode] +=1
            #print(minRuns, encode)
    #print(histogram)
    feature_vec = []
    for scale in range(0,P+2):
        feature_vec.append(histogram[scale])
    #print(feature_vec)
    return feature_vec
```


```python
training_feature = []
for xiter in range(0,5):
    for yiter in range(0,20):
        training_feature.append(LocalBinaryPattern(training_images[xiter][yiter]))
   
```


```python
testing_feature = []
for xiter in range(0,25):
    testing_feature.append(LocalBinaryPattern(test_images[xiter]))
```


```python
# choice 1 if using KL divergence metric
np_training_feature_norm = [np.array(training_featurelist)/sum(training_featurelist) for training_featurelist in training_feature]
np_testing_feature_norm = [np.array(testing_featurelist)/sum(testing_featurelist) for testing_featurelist in testing_feature]

# choice 2 if using Euclidean norm metric
#np_training_feature_norm = training_feature
#np_testing_feature_norm = testing_feature

# save it in a library as train and test images
# Run KNN on each image to pick K nearest neighbours in the dataset
kneighbours = 3
np_training = np.array(np_training_feature_norm)
np_testing = np.array(np_testing_feature_norm)
np_training_label = np.array([int(x/20) for x in range(0,100)])
np_testing_label_groundtruth = np.array([int(x/5) for x in range(0,25)])

# initialization
np_testing_label = np.zeros(len(np_testing),dtype = int)
np_testing_label_kl = np.zeros(len(np_testing),dtype = int)
distance_metric = np.zeros((len(np_testing), len(np_training)))
kl_divergence = np.zeros((len(np_testing), len(np_training)))


for testsample in range(0, len(testing_feature)):
    for trainingsample in range(0, len(training_feature)):
        distance_metric[testsample,trainingsample] = np.linalg.norm(np_testing[testsample]-np_training[trainingsample])
        kl_divergence[testsample,trainingsample] = np.sum(np.multiply(np_testing[testsample],(np.log(np_testing[testsample])- \ np.log(np_training[trainingsample]))))
    arguments = distance_metric[testsample,].argsort()[:kneighbours]
    klarguments = kl_divergence[testsample,].argsort()[:kneighbours]
    # Assign label to the majority of neighbours
    top_k_label = np_training_label[arguments]
    top_k_label_kl = np_training_label[klarguments]
    unique, counts = np.unique(top_k_label, return_counts=True)
    np_testing_label[testsample] = unique[np.argmax(counts)]
    uniquekl, countskl = np.unique(top_k_label_kl, return_counts=True)
    np_testing_label_kl[testsample] = uniquekl[np.argmax(countskl)]
```


```python
import pandas as pd

df_confusion = pd.crosstab(np_testing_label_groundtruth, np_testing_label, rownames=["Actual"], colnames=["Predicted"], margins=True)
df_conf_norm = df_confusion / df_confusion.sum(axis=1)

df_confusion_kl = pd.crosstab(np_testing_label_groundtruth, np_testing_label_kl , rownames=["Actual_kl "], colnames=["Predicted_kl "], margins=True)
df_conf_norm_kl = df_confusion_kl / df_confusion_kl.sum(axis=1)
```


```python
print(df_confusion)
```

    Predicted  0  1  2  3  4  All
    Actual                       
    0          5  0  0  0  0    5
    1          0  3  0  2  0    5
    2          0  2  3  0  0    5
    3          0  3  0  2  0    5
    4          0  1  0  1  3    5
    All        5  9  3  5  3   25
    


```python
print(df_confusion_kl)
```

    Predicted_kl    0  1  2  3  4  All
    Actual_kl                         
    0               5  0  0  0  0    5
    1               2  1  0  1  1    5
    2               1  1  3  0  0    5
    3               2  0  0  3  0    5
    4               0  1  0  1  3    5
    All            10  3  3  5  4   25
    


```python

```
