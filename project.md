---
layout: page
title: Projects
permalink: /projects/
---

This is a showcase of my academic adroitness and contributions for the love of technology, design and code...
<hr style="border:2px solid">

## [Bias mitigation for equitable learning](https://github.com/rohandubey/)
<p align="center" width="100%">
    <img width="80%" src="portfolio-0.jpg ">
</p>
We use continual learning approaches in which the model is sequentially fine-tuned for each group in a way that preserves the knowledge gained from preceding groups. We used MultiNLI oepn-source and Alexa NLU internal dataset for our inferece tasks. We have used Elastic Weight Consolidation (EWC) and Memory-aware Synapes (MAS) techniques and also presented few combinations of them which outperformed the existing models and also the baseline models. We have given extensive reasoning for the use of continual learning as a method to tackle algorithmic bias. This helps in generation of unbiased fair AI. Thisproject has been submitted to external conferences to be reviewed.
{: style="text-align: justify"}
### Built With:
- DistilBERT - a small, fast, cheap and light Transformer model trained by distilling Google's BERT to do the NLP inference tasks.
- PyTorch - Model training/detection Framework.
- numpy - Mathematical calculation librabry.
- matplotlib - Graph plotting library.
{: style="text-align: justify"}
<p class="last-edit">Project update: Nov 2022.</p>
---

## [Face Mask Detector](https://github.com/rohandubey/Face-Mask-Detector)
<p align="center" width="100%">
    <img width="80%" src="portfolio-1.jpg ">
</p>
In the amidst of COVID-19, as there is no proper channel to effectively monitor people wearing mask and not. So to tackle this problem, I made an AI solution based on YOLOv5 model to detect those who are **not wearing mask**. This software is tested by Rajasthan Police (Govt. of India), and is designed to be installed alongside their CCTVs for efficient working. This software can **multi-stream** many sources together and collects data contionuously from those streams.Based on **YOLOv5** model and **RetinaNet Architecture**, having an accuracy of 98% and average FPS of nearly 300. This project has a easy-to-use GUI and can runon a variety of sources like images, videos, directory, glob, http/rtsp/rtmp streams simultaneously.
{: style="text-align: justify"}
### Built With:
- YOLOv5 - Object Detection Algorithm
- OpenCV - Video and image capturing and streaming library.
- Tkinter - GUI toolkit for Python.
- PyTorch - Model training/detection Framework.
- Batch/Bash - Execution of programming languages for Windows/Linux respectively.
- onnx - An ecosystem for interchanging different ML Model Frameworks.
{: style="text-align: justify"}
<p class="last-edit">Project update: Jan 2020.</p>
---

## [Heart-rate Monitoring System](https://github.com/rohandubey/Pulse-rate-monitoring-system)
<p align="center" width="100%">
    <img width="80%" src="portfolio-3.jpg ">
</p>
This is a software that detects the pulse of a an individual using a webcam or Network IP camera (http/ rtsp/ rtmp). This is a non-touch based methodolody specifically for those people suffering from Hypersensitivity. This project was extremely useful in the COVID-19 pandemic as it works in a contactless manner. Combining OpenCV framework and using Signal transformation techniques, we were able to get the pulese value for the give individual. A basic neural network model is used to detect the facial featurea and rest of the calculations are purely mathematical.
{: style="text-align: justify"}
### Built With:
- dlib - Face detection algorithm.
- OpenCV - Video and image capturing and streaming library.
- Batch/Bash - Execution of programming languages for Windows/Linux respectively.
- scipy - Signal processing techniques like fouier transformation and band-pass filters for pulse extarction.
- numpy - Mathematical calculation librabry.
{: style="text-align: justify"}
<p class="last-edit">Project update: Jan 2020.</p>
---

## [Object detection using homography techniques](https://github.com/rohandubey/Homography-Object_detection)
<p align="center" width="100%">
    <img width="80%" src="portfolio-5.jpg ">
</p>
Homography is technique involving mapping one image to another based on their corresponding related good features. In this project, we can find out the given image from the video feed provided real-time. This project involves feature detection using SIFT algorithm and images are matched by FLANN and K- Nearest Neighbor Techniques.
{: style="text-align: justify"}
### Built With:
- OpenCV - Video and image capturing and streaming library FLANN and kmeans algorithms implementation.
- numpy - Mathematical calculation librabry.
{: style="text-align: justify"}
<p class="last-edit">Project update: Dec 2020.</p>
---

## [Object tracking using Optical techniques](https://github.com/rohandubey/Object-Tracking-OpenCV)
<p align="center" width="100%">
    <img width="80%" src="portfolio-7.jpg ">
</p>
In this project I have covered 6 types of Optical Image Tracking based on the image namely : 
1. **Optical Flow** - *Lucas-Kanade technique (shi-tomasi technique)* which identifie the flow of vectors based on image's interesting features (Point Tracking).
2. **Dense Optical Flow** - *Gunner Farneback's algorithm* which considers entire image to identify flow by taking in account each pixel (Motion Tracking).
3. **MeanShift** - Related to clustering algorithm using *Kernel Density Estimation (KDE)*. Area-wise mean of pixel density is calculated which defines the motion of pixel change.
4. **CamShift** - *Continuously Adaptive Mean Shift (CamShift)* in addtion to MeanShift considers the relative moment of the entire image as comapred to the area o interest.
5. **Single Object Tracking** - Basic CNN and GoTurn model based algrithm for singular object tracking.
6. **Multi Object Tracking** - Extension of Single object tracking with multiple object of interest.
{: style="text-align: justify"}
### Built With:
- OpenCV - Video and image capturing and streaming library.
- numpy - Mathematical calculation librabry.
- cascade classifiers - Haar cascade algorithm that detect objects in images, irrespective of their scale in image and location.
- GoTurn Model - Object tracking model made using regression networks.
{: style="text-align: justify"}
<p class="last-edit">Project update: Jan 2020.</p>
---

## [Image feature detection](https://github.com/rohandubey/Feature_Detection)
<p align="center" width="100%">
    <img width="80%" src="portfolio-6.jpg ">
</p>
Feature Detection is very important understanding an image and marking out the point/region in an image responsible for bringing out the good features of an image. This project contains different methods/approaches how feature detection can be implemented. The methods are:
{: style="text-align: justify"}
1. **FAST Algorithm for Corner Detection** - FAST Algorithm for Corner Detection is a high speed algorithm detects features based on circular intensity, threshold value in a given pixel radius and p roportion of them are stored, then one with 'max' and 'min' intensity are compared to get the feature corner.
2. **Harris Corner Detection** - Harris Corner Detection algorithm is based on difference of intensity for displacement in all directions.
3. **Shi-Tomasi Corner Detection** - This is a faster algorithm similar to Harris corner Detection but instead of diffrence of intensity it chooses minimum intensity and thus reducing the overall compuitation.
4. **SIFT** - SIFT (Scale-Invariant Feature Transform) is a rotaion and scale invariant feature detector and thus is helpful as it maks out he boundary of the pixel circle selected and the freature selection based on scaling can be done.
{: style="text-align: justify"}
### Built With:
- OpenCV - Video and image capturing and streaming library.
- numpy - Mathematical calculation librabry.
- matplotlib - Graph plotting library.
{: style="text-align: justify"}
<p class="last-edit">Project update: Jan 2020.</p>
---

## [Image Data labelling software](https://github.com/rohandubey/Label-Data-Helper)
<p align="center" width="100%">
    <img width="80%" src="portfolio-8.jpg ">
</p>
A Program that creates a bounding box that enables you to construct a predictable pipeline of high-quality training data that will teach your ML/DL-powered computer vision system to find and identify objects in image and video data. The output is generated in a COCO format, which can be further changed into the required format such as PascalVOC, MS-COCO, OID, KITTI, Retinanet, Imagenet, etc. This is a Simple GUI implementation to which the inputs are the input and output data folders. Thew bounding box creation is done in a clickable manner. It supports multiple bounding boxes for a single image. Addtional support for redo image selection, automatic next image display and skipping an image is also implemented.
{: style="text-align: justify"}
### Built With:
- OpenCV - Video and image capturing and streaming library.
- numpy - Mathematical calculation librabry.
- matplotlib - Graph plotting library.
- pandas - Reading/storing data library.
{: style="text-align: justify"}
<p class="last-edit">Project update: Jan 2020.</p>
---

## [Numeric Analysis Algorithms](https://github.com/rohandubey/Numerical_Analysis)
<p align="center" width="100%">
    <img width="80%" src="portfolio-2.jpg ">
</p>
A compilation of python/C++ codes on different Numerical Analysis techniques.
* Numerical methods is basically a branch of mathematics in which problems are solved with the help of computer and we get solution in numerical form.
* The codes gives here solve complex mathematical problems which can not be solved easily by analytical mathematics by using simple arithmetic operations and which requires development, analysis and use of an algorithm along with some computing tools.
* These codes are used in Machine learing algorithms fgor Optimization/Encoding/Pre-processing steps.
{: style="text-align: justify"}
Topics/Algorithms Covered are False Point Method, False Position Method, Bistection Method, Newton's Method, Modified Newton's Method, Fixed Point Method, Forward Difference Method, Backward Difference Method, Thomas Method, Gauss–Seidel method, SOR Method, LU decompostion Method, Jacobi Method, Power Method, Inverse Power Method, Richardson Central Difference Method and Muller's Method.
{: style="text-align: justify"}
### Built With:
- numpy (python) - Mathematical calculation librabry for python.
- math.h (c++) - Mathematical calculation librabry for C++.
{: style="text-align: justify"}
<p class="last-edit">Project update: Dec 2020.</p>
---

## [MS-Paint program using OpenCV](https://github.com/rohandubey/OpenCV_Mouse_Interface)
<p align="center" width="100%">
    <img width="80%" src="portfolio-4.jpg ">
</p>
Using OpenCV and pillow packages to make a program like MS-Paint with muose interface enabled.
{: style="text-align: justify"}
### Built With:
- OpenCV - Video and image capturing and streaming computer vision library.
- numpy - Mathematical calculation librabry.
- matplotlib - Graph plotting library.
- pillow - Image manipulation/processing library
{: style="text-align: justify"}
<p class="last-edit">Project update: Dec 2020.</p>
---

## [Project on Facial expression recognition](https://github.com/rohandubey/Facial-Expression-Recognition)
<p align="center" width="100%">
    <img width="80%" src="portfolio-9.jpg ">
</p>
This project implements the real-time facial expression recognition of seven most basic human expressions: ANGER, DISGUST, FEAR, HAPPY, NEUTRAL SAD, SURPRISE built usng basic cnn modules and computer vision techniques.
{: style="text-align: justify"}
### Built With:
- OpenCV - Video and image capturing and streaming computer vision library.
- numpy - Mathematical calculation librabry.
- matplotlib - Graph plotting library.
- tensorflow/keras - Deep learning libraries.
- flask - Micro-web framework for hosting a python code over the net.
{: style="text-align: justify"}
<p class="last-edit">Project update: Jan 2020.</p>
---

<p class="last-edit">Last update: 20 Nov 2022.</p>
