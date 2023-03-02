# blurgs_task
This was my technical round task for an internship position at Blurgs (https://www.blurgs.com/).
I was tasked with annotating data and training a model to identify temperature based anomalies in Solar panels using object detection and semantic segmentation.

<a href="https://docs.google.com/document/d/1tNs_Akz0ZNM_Pgl40J_kac_POlSOWQn58E3SO3A_QkA/edit?usp=sharing"  target="_blank"> Project Report Link </a>

## Overview
The tech stack I used in this task was a new one. I had never used RoboFlow before. 
In addition this was the first time I worked with Object Detection and Semantic Segmentation. This report is to properly convey my thinking process and procedure of working on this task.

## Dataset Creation with Roboflow.
Upon learning what Object detection is, my first and biggest doubt was with regards to data labeling. I wanted to know how one saves the categorization of an object within a larger image. 
Roboflow made the task easier. I was able to label all the images two times in under a day.

I labeled the images in two different styles:
1. Marking only the solar anomaly itself
2. Marking the entire panel (the required style in the task)

In addition, due to the high frequency of the Light Hotspot, I decided to not label those in my dataset and to only move forward with Hotspot and Bypass Diodes. This was due to the fact that Light Hotspots are too frequent and the pattern by which they would be recognized has very high chances of occurring at other places making False Positives a very high likelihood had Light Hotspot been a category too.

Roboflow Project Link - https://universe.roboflow.com/virenn-jays-workspace/blurgs_task_proper

## Pre-Processing Functions
I created two versions of the dataset in Roboflow with varying pre-processing functions.
Both of them had the following common transforms
- Grayscale - Since the image is already grayscale, converting it to an actual grayscale image would make the dataset easier to learn from
- Resize - Resized all the images to 640 x 640 to make the input data uniform.
- Augmentations - Each dataset was also increased to a total of 540 images using the following augmentations
- Random Crop - This would randomly crop a section of any photo at any level of zoom from  0% to 50%. This means that the model would have to learn to identify the panels from various photographs, and not just complete ones with borders etc.
- Random Flip- This function would randomly flip the image horizontally or vertically and add it as a new image in the dataset. This allows for images from various angles
This increased the total number of images to 540.

### The Difference in Datasets
The main difference between the two datasets I generated was the use of the Auto-Adjust Contrast.

Auto-Adjust contrast would bring out the difference between the panel and the anomalies more, and allow the network to learn better. However this only applies in the case of the images that are taken well.

Thus to test out my hypothesis , I created two datasets.

## Train/Val split

For the train/val split, I went ahead with the following split:
85% train and 15% validation
This was due to the fact that the dataset itself is actually pretty small , and the higher the number of images to train on, the better the results would be.
However, it is important to note that due to the augmentation step, the actual split ended up being 96% (510 images) train and 4% (30 images) validation 



## Training with YoloV8
Since this was my first time working with Object Detection , I went ahead with using YoLoV8 which was mentioned in the task description.

There were two reasons for that
YoLoV8 had some great examples, which I could also just modify slightly in-order to be able to train my own version
RoboFlow could export the annotated dataset in a structure supported by YoLoV8, and I could just directly import the dataset without having to download and upload onto kaggle.
