# Data Science - Best Practices &middot; [![License](https://img.shields.io/badge/license-CC%20BY%204.0-blue)](./LICENSE.txt)

## Table of Content

- [Chapter 1 - Introduction](./readme.md#chapter-1---introduction)
- [Chapter 2 - Project Team (Design)](./project_team.md#chapter-2---project-team)
- [Chapter 3 - Architecture (Deploy)](./architecture.md#chapter-3---architecture)
- [Chapter 4 - Source Code (Engineer)](./source_code.md#chapter-4---source-code)
- [Chapter 5 - Documentation (Engineer)](./documentation.md#chapter-5---documentation)
- [Chapter 6 - Versioning (Engineer)](./versioning.md#chapter-6---versioning)
- [Chapter 7 - Data Management (Engineer)](./data_management.md#chapter-7---data-management)
- [Chapter 8 - Dependency Management (Engineer)](./dependency_management.md#chapter-8---dependency-management)
- [Chapter 9 - Configuration Management (Engineer)](./configuration_management.md#chapter-9---configuration-management)
- [Chapter 10 - Testing (Engineer)](./testing.md#chapter-10---testing)
- [Chapter 11 - Quality Measurements (Monitor)](./quality_measurements.md#chapter-11---quality-measurements)
- [Chapter 12 - Model Training (Engineer)](./model_training.md#chapter-12---model-training)
- [Chapter 13 - Distribution (Deploy)](./distribution.md#chapter-13---distribution)
- [Chapter 14 - Cloud-Deployment (Deploy)](./cloud_deployment.md#chapter-14---cloud-deployment)
- [Chapter 15 - Edge Deployment (Deploy)](./edge_deployment.md#chapter-15---edge-deployment)
- [Chapter 16 - Monitoring (Monitor)](./monitoring.md#chapter-16---monitoring)
- [Chapter 17 - Automation (Scalability)](./automation.md#chapter-17---automation)
- [Chapter 18 - Scaling (Scalability)](./scaling.md#chapter-18---scaling)
- [Chapter 19 - Sizing (Scalability)](./sizing.md#chapter-19---sizing)
- [Chapter 20 - Security (Engineer)](./security.md#chapter-20---security)
- [License & Contributing](./license.md)

## Chapter 15 - Edge Deployment

Aside from running your ML models on the cloud you can also use edge deployment.

### Edge Computing Introduction

[![What is Edge Computing? - Video](https://img.youtube.com/vi/cEOUeItHDdo/0.jpg)](https://www.youtube.com/watch?v=cEOUeItHDdo)

### Why Consider Edge Deployment

With your model deployed to the cloud the production input data needs to be transferred to your cloud server and the output data (the prediction) needs to be transferred back to the client.
This has multiple implications:

- A connection to the cloud server is required, often via the Internet.
For some client devices a connection to the cloud is considered a security risk e.g., servers in a very secure environment.
- The input data and also the output data might be of high volume and large size.
This causes the need for enough network capacity.
This may pose a significant cost increase for your users, e.g. for mobile users when using their mobile data connection.
- The data needs to travel from the client to the server, which might not be desired e.g., sensitive information, or even allowed e.g., because of regulations or other legal constraints.
- The transfer of the data takes some time to travel the distance to the server and back.
For some applications this turn-around time might be too large and therefore might back a use-case, e.g. in autonomously driving cars.

By moving the model on to or closer to the client device many of these implications can be reduced or completely avoided.

### Downsides of Edge Deployments

Here is a list of challenges that often come with edge deployments:

- Versioning: Every device will get their own version of the model.
When a new version of your model is released you need to make sure the model upgraded on all devices.
Often this process takes days or weeks; in some cases, it might take year for everyone to upgrade to the latest version; sometimes that actually never happens.
On a server / cloud deployment only the one model on the cloud needs to be switch, which is usually done in seconds, minutes or hours.
- In an edge deployment scenario, there is no (direct) reason to send the production data to the cloud.
This may create the issue that you'll never receive it and can  check the accuracy of your training data.
Generally, your training data will not grow in such a case.
Therefore, an asynchronous feedback loop for the data needs to be added to the solution, e.g. sending some feedback data back to the server over the night.
- The client's devices become part of your solution, over which you might have very little control.
This creates additional dependencies and may even create the requirement to make your solution run on multiple platforms, e.g. Android and iOS for mobile apps.
- In general edge devices are not as "potent" as dedicated cloud servers. Therefore, you might need to optimize your model / solution to run on a small CPU / memory / disk space footprint. It could be difficult to run a neural network with millions of parameters on an edge device. This optimization can be performed with **Model Compression**.

The standard frameworks that are being used for Machine Learning do not have a particular size focus and are therefor not great for deploying models on edge, where the size can be key. To deal with this problem edge deployment can use **Model Compression.** 

### Apple CoreML

Apple offers the [CoreML framework](https://developer.apple.com/documentation/coreml) to let your ML run directly in your iOS application on the user's iPhone or iPad.

> Use Core ML to integrate machine learning models into your app. Core ML provides a unified representation for all models.
> Your app uses Core ML APIs and user data to make predictions, and to train or fine-tune models, all on the user’s device.

### TensorFlow Lite

> TensorFlow Lite is an open source deep learning framework for on-device inference.

[TensorFlow Lite](https://www.tensorflow.org/lite) can be used to deploy TensorFlow models as part of an [Android app](https://developer.android.com/ml) or other IoT devices.

### Model Compression

Model Compression reduces the model in size with different techniques like *Knowledge Distillation*, *Model and Weight Pruning* or *Integer Quantization*. Model Compression should only be used if it is really necessary to reduce the model size. Edge devices like mobile devices or embedded devices nowadays have often enough resources to run the model without the need for Model Compression. Model Compression can be performed with [Knowledge Distillation](https://arxiv.org/abs/1503.02531) or a compression framework like [Learn2Compress](https://ai.googleblog.com/2018/05/custom-on-device-ml-models.html).

>**Model Compression is an architectural decision and should only be used in cases where there's a hard requirement to do so. Do not compress a model just to "have it compressed".**