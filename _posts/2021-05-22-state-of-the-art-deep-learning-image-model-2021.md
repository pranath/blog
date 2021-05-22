---
toc: true
layout: post
categories: [deep-learning-theory]
title: State-of-the-art Deep Learning image model techniques in 2021
description: In this article we are going to look at some of the most advanced techniques available in 2021 for training deep learning vision models.
comments: true
image: images/deep1.jpg
---

## Introduction

In this article we are going to look at some of the most advanced techniques available in 2021 for training deep learning vision models. These go beyond the basics of mini-batch gradient descent, learning rates, pre-sizing, transfer learning, discriminative learning rates, and mixed-precision training.

## Library and Dataset

I will be using the fastai deep learning library for code examples, as well as the fastai curated *Imagenette* dataset which is a specially curated subset of the well known ImageNet dataet of 1.3 million images from 1,000 categories. The Imagenette dataset consists of a much smaller set of images and just 10 categories of images.

We will define a baseline model here using the dataset to then compare the effect of each advanced technique.

![]({{ site.baseurl }}/images/sota-vision/sota-vision1.png " ")

## Normalisation

When training a model, its helpful to ensure the image data is normalised. This ensures that different images end up with data that is in the same range of values, which helps the model better focus more on the content on the images. So here by normalised we mean we want the image data values to have a mean of 0 and a standard deviation of 1.

The fastai library will automatically normalise images per batch, and this is suitable for models that we might train from scratch. When using transfer learning this default approach is not a good idea, because a pre-trained model has been trained on image data with a particular mean and standard deviation. So to use a pre-trained model with new images, we need to ensure these new images are normalised to the same mean and standard deviation that the original model data was trained with.

We can do this my specifying particular normalisation stats in fastai, which already knows the normalisation stats for many common datasets, including of course fastai's own Imagenette dataset which makes it much easier.

We can also define a function **get_dls()** which will make it easier to define different types of data loader i.e. with different batch or image sizes.

![]({{ site.baseurl }}/images/sota-vision/sota-vision2.png " ")

After applying our normalisation, we can see the mean and standard deviation are approximatly 0 and 1 respectively on a test batch of images.

Lets now try this normalised data and train our model.

![]({{ site.baseurl }}/images/sota-vision/sota-vision3png " ")

While normalisation here hasn't made a huge improvement over our baseline model, normalisation does make a bigger difference especially with bigger models and more data.

## Progressive resizing

## Test time augmentation

## Mixup

## Label smoothing

## Conclusion
