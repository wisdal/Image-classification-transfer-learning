## A Custom Image Classifier using Transfer Learning on Google Inception V3

When we think of multi-class image classification, we usually consider building our model from scracth for best fit, we say. This is one option. But building a deep learning model can take weeks, depending on your training dataset and the configuration of your network (your deep learning network).

But did you know there already exists some models which perform pretty well in classifying images from various categories? Then you may have already heard of [ImageNet](http://www.image-net.org/), and its Large Visual Recognition Challenge. This is a standard task in computer vision, where models try to classify entire images into 1000 classes, like "Zebra", "Dalmatian", and "Dishwasher". 
[Inception V3](https://research.googleblog.com/2016/03/train-your-own-image-classifier-with.html) is such a model. Released in 2015, it is a research product of Google Brain Team.

Can we take advantage of this existing model for a distinct image classification task ? Well, the concept has a name: [Transfer learning](https://en.wikipedia.org/wiki/Transfer_learning). It may not be as efficient as a full training from scratch, but is surprisingly effective for many applications. It allows model creation with significantly reduced training data and time by modifying existing rich deep learning models.

## What ?

We will demonstrate the effectiveness of using Transfer Learning on Inception V3 with a custom task : building a powerful image classifier to categorize products images (Food & Grocery). This is to help a retailer reduce the amount of human effort in its warehouse and retail outlets. 

Problem Statement from [Hackerearth](https://www.hackerearth.com/fr/problem/machine-learning/identify-the-objects/description).

[Download Dataset](https://he-s3.s3.amazonaws.com/media/hackathon/deep-learning-challenge-1/identify-the-objects/a0409a00-8-dataset_dp.zip)

## How ?

We will build our classifier in 5 steps :

1. Preprocess dataset (set up image folder, data augmentation, etc.).
2. Download the Inception V3 [pre-trained model](http://download.tensorflow.org/models/image/imagenet/inception-v3-2016-03-01.tar.gz). The retrain script will download it by default.
3. Retrain the Bottleneck. That sounds weird right :) ?
4. Fine-tune the model.
5. Test the model.

By the way, "Bottleneck" is just the informal name given to the last hidden layer of a deep neural network.

## Why does it work ?

In a neural network, neurons are organized in layers. Different layers may perform different kinds of transformations on their inputs. Signals travel from the first layer (input), to the last (output), possibly after traversing the layers multiple times. As the last hidden layer, the Bottleneck has enough summarized information to provide the next layer which does the actual classification task.

In the [retrain.py](https://github.com/wisdal/image-classification-transfer-learning/blob/master/retrain.py) script, we remove the old top layer, and train a new one on the pictures we have downloaded.

The reason our final layer retraining can work on new classes is that it turns out the kind of information needed to distinguish between all the 1000 classes in ImageNet is often also useful to distinguish between new kinds of objects.

## Step by step

Hold your excitement for a little while... the path is thorny, comrade!

#### Data Preprocessing

The downloaded dataset comes with a train folder that we need to set up properly. Our goal is to put each of the images in a subfolder representing its category. At the end, we will have x subfolders, With x the number of distinct categories.

For this preprocessing purpose, I provide you with the [pre_process.ipynb](https://github.com/wisdal/image-classification-transfer-learning/blob/master/pre_process.ipynb) notebook.

#### Retraining the Bottleneck and Fine-tuning the model

Courtesy of Google, we have the retrain.py script to start right away.

We just need to configure some basic parameters:
- image_dir: path to folder of labeled images.
- output_graph, intermediate_output_graphs_dir, output_labels, etc.: where to save output files.
- distortion features.
- ...
- how_many_training_steps: number of epochs.
- learning rate.
- ...

Fill free to play with these parameters, given you know  what you are doing. Remember that you can use the TensorBoard to visualize the resultS of your training.

Learning rate, nb. of epochs, etc. are deterministic parameters. Use them to fine-tune your model.

You may be able to get ~85% of accuracy to start with.

#### Test the model on unseen records

Once you are satisfied with the model you built, test it on the unlabelled images in test folder.
Check out my [test.ipynb](https://github.com/wisdal/image-classification-transfer-learning/blob/master/test.ipynb) notebook for live predictions on the test data.

You may also submit your prediction in a csv file on the Hackerearth platform. Give it a try and share your performance.

## Requirements
- Python >= 3.4 
- TensorFlow

Feel free to contact me: [wdalmeida6@gmail.com](mailto:wdalmeida6@gmail.com)

## Resources
[tensorflow.org](https://www.tensorflow.org/tutorials/image_recognition)

