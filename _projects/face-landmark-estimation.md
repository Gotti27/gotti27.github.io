---
layout: page
title: Face landmarks estimation
toc:
  # beginning: true
  sidebar: left
# description: Solving face landmark estimation with DL
img: https://wywu.github.io/projects/LAB/support/WFLW_annotation.png
importance: 3
category: Computer Vision
related_publications: false
---

- [Github repo](https://github.com/Gotti27/face-landmark-estimation)

## Introduction

The goal of this side project is to create and train a model to estimate the location of 98 face landmarks.
These landmarks can be used for head pose estimation, sentiment analysis, action recognition, face filtering and more.

## Dataset

As dataset, I choose the Wider Facial Landmarks in-the-wild ([WFLW](https://wywu.github.io/projects/LAB/WFLW.html))[^1] from Wayne Wu et al., containing a total of 10000 annotated faces (7500 for training and 2500 for testing).
The following figure shows where the landmarks are located:
![wflw-dataset](https://wywu.github.io/projects/LAB/support/WFLW_annotation.png)

### Training Pipeline

For the first implementation of this model, I assumed that the input image will already be cropped around each face by another algorithm.
Next models might take directly as input the full image without face pre-detection.

The dataset provides along each landmarks annotation list, the corresponding face bounding box.
Hence, each training image is first cropped (while still keeping a padding mask around the box), converted into 32bit float, resized to 224x224, and lastly normalized with a mean equal to 0.5.

## Models

Although heatmap-based methods seems to be more accurate in this type of tasks, I decided to start with a traditional convolutional architecture.
Based on results and future developments, other approaches may be taken into consideration.

|  model   | test loss |
| :------: | :-------: |
| ResNet50 |   10.1    |

### First Version

The first version's architecture is a standard CNN architecture using ResNet50[^2] as backbone.
So, the last ResNet layer has been changed to produce a 196 dimensional output tensor, i.e. _(x,y)_ for each one of the 98 markers.
Such model has been trained for 200 epochs using Wing loss[^3].

Model 1 outputs on some test set elements:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/face-landmark/model-1-test.jpg" title='model1-test' class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The following plot contains training loss (blue) and validation loss (red):

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/face-landmark/model-1-train-val-loss.svg" title='model1-loss' class="img-fluid rounded z-depth-1" %}
    </div>
</div>

[^1]: [Look at Boundary: A Boundary-Aware Face Alignment Algorithm](https://doi.org/10.48550/arXiv.1805.10483)
[^2]: [Deep Residual Learning for Image Recognition](https://doi.org/10.48550/arXiv.1512.03385)
[^3]: [Wing Loss for Robust Facial Landmark Localisation with Convolutional Neural Networks](https://doi.org/10.48550/arXiv.1711.06753)
