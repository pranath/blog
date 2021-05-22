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

We will define a baseline model here using the dataset to then compare the effect of each advanced techniques.

![]({{ site.baseurl }}/images/sota-vision/sota-vision1.png " ")

## Normalisation

When training a model, its helpful to ensure the image data is normalised. This ensures that different images end up with data that is in the same range of values, which helps the model better focus more on the content on the images. So here by normalised we mean we want the image data values to have a mean of 0 and a standard deviation of 1.

The fastai library will automatically normalise images per batch, and this is suitable for models that we might train from scratch. When using transfer learning this default approach is not a good idea, because a pre-trained model has been trained on image data with a particular mean and standard deviation. So to use a pre-trained model with new images, we need to ensure these new images are normalised to the same mean and standard deviation that the original model data was trained with.

We can do this my specifying particular normalisation stats in fastai, which already knows the normalisation stats for many common datasets, including of course fastai's own Imagenette dataset which makes it much easier.

We can also define a function **get_dls()** which will make it easier to define different types of data loader i.e. with different batch or image sizes.

![]({{ site.baseurl }}/images/sota-vision/sota-vision2.png " ")

After applying our normalisation, we can see the mean and standard deviation are approximatly 0 and 1 respectively on a test batch of images.

Lets now try this normalised data and train our model.

![]({{ site.baseurl }}/images/sota-vision/sota-vision3.png " ")

While normalisation here hasn't made a huge improvement over our baseline model, normalisation does make a bigger difference especially with bigger models and more data.

## Progressive resizing

Progressive re-sizing is another technique pioneered by fastai. Essentially this involves training models on smaller versions of the images first, before continuing training on bigger images. This has 2 major benefits:

- Model training time is much faster
- Model accuracy ends up better than if we trained the model only on bigger images

How can this be the case? lets remember that with convolutional deep learning models, early layers focus on recognising primitive features like lines and edges, and later layers more composite features such as eyes or fur. So if we change the image size during training, our earlier model will still have learnt many useful things applicable to bigger and higher resolution images.

In a way, this is a bit like training a model in one area then re-using that model on a similar area - which might sound familiar? As it should since this is very much what transfer learning is about as well, which works very well. So we should perhaps not be so surprised that this could work well.

Another benefit of using lower resolution/smaller versions of the images first is that this is another kind of data augmentation - which should also help our models generalise better.

So lets use our *get_dls()* function that we defined earlier to define a data loader for our smaller lower resolution images and train the model for a few epochs.

![]({{ site.baseurl }}/images/sota-vision/sota-vision4.png " ")

We will the define a new data loader with bigger images, and continue to train our model with these.

![]({{ site.baseurl }}/images/sota-vision/sota-vision5.png " ")

So we can see we are already getting much better results than our baseline with just a few epochs, and much more quickly. It's worth considering for the desire task, for transfer learning can in some cases harm performance. This might happen for example if the pre-trained model is trained on images already quite similar to the new ones you want to recognise - as in this case the model parameters are likely already quite close to what is needed, and progressive resizing could move the parameters further away from this and good results. If the use case is for the pre-rained model is very different to what it was originally trained on i.e. very different sizes, shapes, styles etc - then progressive resizing here might actually help.

In either case, trying things experimentally would probably be the best way to determine which was the better approach.

## Test time augmentation

## Mixup

## Label smoothing

## Conclusion
