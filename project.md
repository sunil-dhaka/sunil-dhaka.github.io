---
layout: page
title: Projects
permalink: /projects/
---

This is to showcase some of my projects
<hr style="border:2px solid">

## [Bias mitigation for equitable learning](https://github.com/rohandubey/)
<p align="center" width="100%">
    <img width="80%" src="portfolio-0.jpg ">
</p>
We use continual learning approaches in which the model is sequentially fine-tuned for each group in a way that preserves the knowledge gained from preceding groups. We used MultiNLI oepn-source and Alexa NLU internal dataset for our inferece tasks. We have used Elastic Weight Consolidation (EWC) and Memory-aware Synapes (MAS) techniques and also presented few combinations of them which outperformed the existing models and also the baseline models. We have given extensive reasoning for the use of continual learning as a method to tackle algorithmic bias. This helps in generation of unbiased fair AI. Thisproject has been submitted to <hr style="border:2px solid">
<hr style="border:2px solid">
l conferences to be reviewed.
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