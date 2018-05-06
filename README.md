# Vehicle Registration Plate Detection and Recognition

## Abstract


In this project, we aim to build an automated pipeline to perform license plate detection from images of vehicles and perform recognition of characters in the number plate. This would involve first localizing the number plate in the image followed by recognizing the characters on the plate.

## Requirements

1. MATLAB version 2015b
2. Python 3.6 (To generate synthetic data)


## Dataset

The dataset is available in the following link:
https://drive.google.com/open?id=1Zv2eksvwpLqn5m2LPk38qASamnVtt7aQ


## Solution Approach

### Number Plate Localization

1. Preprocessing

In this process we sharpen the image and then convert it to gray scale. Finally we binarize the image.

2. Localization

#### Approach-1

Edge Detection We perform edge detection in order to find the closed regions in the image which are close to the shape and size of a number plate.

Morphological Closing Since we need closed regions, so we perform morphological closing on the edge detected image.

Detection of ROI From the closed regions obtained in previous step we need to find the candidates for the number plate. To do this we use bounding boxes which satisfy properties specific to a number plate

Problems:- 
1. Lots of Hyperparameters so tuning was difficult.
2. Although the approach was producing decent results on an average but it was not able to give exact and clean results as well as the percentage of false positives was high.
3. More restrictive model due to high dependence on heuristical properties of a number plate

#### Approach-2

We modified Approach-1 to overcome some of its problems as discussed above. The key idea behind this approach is that the size of a number plate is fixed when considered for a particular country in general. In order to figure out the regions in the image of this particular size, we follow image difference method.

Steps:- 
1. Removal of objects which are very small or very large as compared to the known size of the number plate. This gives us Image1.
2. We remove the objects from the Image1 which are of comparable size to the number plate. This gives us Image2. 
3. Taking the difference of Image1 and Image2 we obtain the candidate number plates. 
4. We use circularity i.e how similar the shape of an object is to a circle (in our case we need it to be close to a rectangle) to get the final output as the number plate

This method reduces dependency of finding the number plate on a large number of properties of a number plate. Moreover, it has lesser number of false positives as compared to Approach-1.

### Character Recognition

We extract the characters from the localized number plate before doing the recognition so as to make the recognition step less complex.

#### Segmentation 
For segmenting characters from the localized number plate, we use method of vertical projections. Here we want to find out the spaces which separate the consecutive characters in the number plate. These spaces are used to split the number plate resulting into regions having individual characters.

#### Character Recognition 
In this step we attempted following methods: 

Template based classification:

1. Single instance matching In this method, the test image is matched with templates of characters ranging from A-Z and digits 0-9 using similarity measures. The test image is recognized as the character represented by the template having highest similarity with it.

2. Multiple instance matching In this method, we synthetically generated character templates of different fonts. The similarity is calculated for a test image with respect to either all the instances or the average of them.

3. k-Nearest Neighbor In this method, we use the inbuilt kNN classifier to train the model and then we predict the character using this model.

All of the above mentioned methods work well for clean and properly formatted data.

Neural Nets:

Due to lack of sufficient data to train and constraints of MATLAB 2015, we could not get results for this method.

## Instructions for Running the Code
1. To perform Localization: run LocalizationDemo.m. 

#### Input: 
Car image (Some sample images can be found in the directory named Samples)
#### Output:
Number plate in the image

2. To perform character segmentation: run SegmentationDemo.m . 

#### Input:
A numner plate image (You can use images generated by Localization from Localization Results directory)
#### Output:
Images containing characters in input image.

3. To perform character recognition: run RecognitionDemo.m. 

#### Input:
A binary image having a character (You can use images generated by Segmentation from Segmentation Results directory)
#### Output:
Character in the image.


## Results

Image | Localization
--- | ---  
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Samples/G1%20(5).jpg)  | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Localization%20Results/G1%20(5).jpg)
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Samples/G2%20(163).jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Localization%20Results/G2%20(163)_1.jpg)
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Samples/G2%20(164).jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Localization%20Results/G2%20(164)_1.jpg)


Image | Segment1 | Segment2 | Segment3 | Segment4 | Segment5 | Segment6
--- | --- | --- | --- | --- | --- | ---
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Localization%20Results/G1%20(5).jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G1%20(5)/1.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G1%20(5)/3.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G1%20(5)/4.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G1%20(5)/5.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G1%20(5)/6.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G1%20(5)/7.jpg)
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Localization%20Results/G2%20(163)_1.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(163)_1/1.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(163)_1/2.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(163)_1/3.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(163)_1/4.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(163)_1/5.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(163)_1/6.jpg)
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Localization%20Results/G2%20(164)_1.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(164)_1/1.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(164)_1/2.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(164)_1/3.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(164)_1/4.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(164)_1/5.jpg) | ![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Segmentation%20Results/G2%20(164)_1/6.jpg)

Image| OCR
---| ---
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Sample%20Characters/2.jpg) | 2
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Sample%20Characters/A.jpg) | A
![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Sample%20Characters/O.jpg) | O


### Accuracy (%)

Segmentation depends on how the plates are extracted. So, if a plate is extracted correctly, the segmentation into characters works.

Data Set | Total Images | Localization | Segmentation 
--- | --- | --- | ---
G1| 810 | 55.95 | 55.95
G2| 700| 54.85 | 54.85
G3| 743  | 49.79 | 49.79
G4| 572 | 31.81 | 31.81
G5| 1152 | 51.73 | 51.73

Method | OCR
--- | ---
Single Instance | 27.77

Because of noisy data and inconsistent format in test images and template images, we are not able to accurate results.

### Sample Image for which the method fails

![Screen Shot](https://github.com/bhatsukanya/Number-Plate-Detection-and-Recognition/blob/master/Samples/G4%20(5).jpg)

This is due to bad lighting condition and lots of noise in the image.
