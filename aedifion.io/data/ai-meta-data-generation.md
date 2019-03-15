---
description: The concept of aedifion.io's AI-based meta data generation
---

# AI meta data generation

## Problem Description

Since there a usually hundreds or thousands of datapoints within a building it is often very difficult to find structure in the heap of data. Additional meta data read from e.g. BACnet devices may help to add form, but might also be wrongly annotated. Therefore, we provide you with an AI system, that will analyze the obtained data from your buildings and provide annotations on what the datapoints represent. Each week this service provides you with an updated estimate on your building's datapoints.

## Supervised Learning

The method we apply for creating such an AI system is _supervised learning_. This methods creates a model based upon training data examples and their known annotations \(called training set\). The model is basically a parameterized function from the input data to a probability distribution over the target labels. Each model uses the training set as input, then optimizes its parameters to match the known annotations best \(called training\). While optimizing it aims at reducing the probability of wrongly classifying an example on the training set.

The measure of performance of such a classifier is the _accuracy_, i.e. the percentage of examples which are classified correctly. After the training of the classifier its performance is evaluated on another dataset with known annotations called the test set. The model did not see any of these examples before. Therefore, the measure of accuracy on the test set should give an indication on how well the model performs on unknown data.

The known annotations are a set of 22 classes. They describe the type of datapoint in your building. A [complete list of all of the classes and their explanation](https://docs.aedifion.io/docs/engineers/specifications/artificial-intelligence) is available.

## Deep Learning

Within our AI based classification system we use state-of-the-art deep neural networks. Neural networks are a machine learning model, that was designed to behave similar to the human brain. It is comprised of multiple layers in which the output of the previous layer is transformed. In recent years the performance of computers did increase to that point, that training and using large \(i.e. deep\) neural networks has become feasible. Deep neural networks generally provide very good performance and can generalize very well to unknown data. One type of deep neural network is the Convolutional Neural Network \(CNN\). It uses trainable convolutions in each layer.

![A convolutional neural network \(for images\) \[CC BY-SA 4.0, Source: Aphex34\]](../../.gitbook/assets/cnn.png)

## Results

The aedifion.io AI based classification system was trained on a year's worth of data from our building. In our evaluation 82.64 % of the examples in our test set were classified correctly.

## Classification Service

Aedifion provides the AI based classification service as a regular \(usually weekly\) analysis on your building. During each analysis, the observations are collected, the model is applied and the datapoints are augmented with updated meta data tags.

## Continuous Learning

Since the classification system works with statistical models, it might classify a datapoint wrongly. To improve future classification accuracy you can disapprove an AI annotation of a datapoint. With this you not only help the AI system directly, but also improve the quality of data such that the AI system keeps improving.

