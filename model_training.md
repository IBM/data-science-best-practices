---
layout: default
title: Model Training
nav_order: 12
---
# IBM Data Science - Best Practices

## Model Training

### Resource intensity

Model training can be a computationally expensive task.
The complexity stems from algorithm choice and data characteristics (data complexity and dataset size).
During training, large amounts of data need to undergo several stages of processing (e.g. multiple layers of a Neural Network).
As a result, model training is a resource intensive step.

The complexity of the model can be described using the Big O notation.
For example training of a sklearn implementation of SVM classifier scales between *O(d n<sup>3</sup>)* and *O(d n<sup>4</sup>)* where *d* is the number of features and *n* is the dataset size.
As a Rule of Thumb, the larger the number of parameters, the greater the complexity of the algorithm.
In case of complex algorithms such as Neural Networks, other challenges relating to the stability of the model optimization come into place.
Issues such as dead neurons and disappearing or exploding gradients, increase the number of training iterations needed to reach a stable minima.

The characteristics of the data also play a role in determining the computational resource intensity of the training.
With increase in number of features and number of examples in the dataset, the amount of data that needs to be processed increases.
Noisy features can also cause the number of iterations needed to reach a minima to increase, resulting in more computational resource usage.
  
### Training considerations

There are many different scenarios and requirements to model training - some of them are described here:

- **one-off vs. continuous:** A model can only be trained once and put to production, where it will not be touched anymore.
But as with predictions and respective possible corrections or new data there is new input for the model, it might be retrained to yield better performance.
If this is continued very frequently the model training is called continuous.

- **GPU vs CPU vs TPU:** There are different technical configurations for hardware, which runs machine learning model training.
The typical unit for calculation is a CPU.
For most deep-learning use cases a GPU may have better performance though as  massive matrix multiplication is needed.
GPUs' bandwidth for memory transfer is much higher and with thread parallelism much more effective for big chunks of memory units (as matrices are).
TPUs are specially tensorflow processing units.
They are specifically shaped for processing tensorflow models.

- **real-time vs. mini-batch vs. batch:** There will be use cases where there is new data frequently arriving, which can be used for retraining the model and achieve higher performance.
This incoming data can either be used immediately for retraining as it arrives (real-time), there might be a small threshold for an amount of records to arrive (1000+) or data needs to be present as a large batch and the model is only trained then.

- **Training on the Cloud:** It offers a large advantage when running model trainings on the cloud.
Most machines can be booked and allocated on demand, therefore you only pay for the time the model training is running.
In addition to that opportunities to scale are almost limitless.
As with more resources most jobs also run faster, there is also lower cost due to smaller run times, which covers the higher costs of the infrastructure.
Most cloud providers offer GPUs and TPUs on demand for training.
With managed services (AWS Sagemaker, Watson Studio) the process of model training is even more simplified.
So data scientists can focus on their main objectives.
As there is a limitless amount of machines available most trainings can also be run in parallel, which is very relevant for hyperparameter tuning.
As the same model is trained with different input parameters to find the optimal set of parameters.

- **Edge vs Centralized:** Edge devises refer to devices through which data enters the network.
This typically refers to mobile or IoT devices.
These devices have significantly smaller computational resources compared to local machines, in-house servers, or cloud.
However, there are cases where training / inference of machine learning models is desired on such devices, typically due to connectivity and latency requirements.

### Training approach

The selection of the *"correct"* training approach based on the considerations discussed above is use-case specific.
The guidelines below are intended to aid in some of the decision-making.

**Business considerations:**
Beyond technical considerations, business considerations may also dictate the training approach.
There may be some restrictions on where data can reside.
For example, it may not be possible to move the data outside of client's IT infrastructure.
In which case, the training has to be run either in-house or through client's cloud provider services.
If retraining or continuous training is considered, the training approach may need to be managed by the client's team.
In such cases, client's team capabilities also need to form part of consideration.
Additional guidance or use of more managed service may be needed.  

**Where to train?**
Models can be training locally or on a server / cloud.
Edge training typically does not replace centralised training so it is discussed separately below.
Simpler models for which the local machine's resource are sufficient may be trained locally before deployment to location where inference is run.
This should be run inside a docker container to ensure compatibility with the eventual deployment environment.
When resource demands are beyond local machine capability or models that are expected to be retrained, training on server / cloud is needed.

Kubernetes jobs are designed for running batch processes such as model training on Kubernetes clusters.
Kubernetes jobs are higher level of abstraction constructs compared to Kubernetes Pods.
Jobs handle the deployment of specified number of Pods and restarts faulty Pods.
Number of Pods indicate the number of parallel training tasks that can be run.
If more models need to be trained, the number of Pods can be increased in Job description.
If the number of models exceed the number of pods, a queue is implemented.
Multiple models may be trained at the same time when hyperparamter optimization is done.
Upon completion, the pods are not terminated.
Pods are terminated if the job is deleted.
<br>*(For more details and an example see [here](https://mlinproduction.com/k8s-jobs/))*

If training is done using Cloud provider's managed service, the equivalent job set-up can be done using the native tools.

**What to train on?**
When selecting the hardware to run on, it is important to consider the capabilities of the implementation of the algorithm.
For example, using a node with more CPUs will not improve the processing speed of an algorithm that is not parallelized.
For typical sklearn algorithms, [joblib](https://joblib.readthedocs.io/en/latest/) library can be used to enable parallel processing.
For the use of TPUs, the compatibility of the Tensorflow algorithm can be checked using [Tensorboard](https://www.tensorflow.org/tensorboard).
The Graph tab can be toggled to show TPU incompatible components of the model.
When working with large datasets, it is also important to consider RAM usage (See [here](https://scikit-learn.org/stable/developers/performance.html#memory-usage-profiling) for memory profiling).

**Should Edge training be used?**
Due to limited computational resources available on edge devices, only the simple algorithms can be trained on edge devices.
For more complex algorithms, transfer learning is often used.
The majority of training is done in centralised location and the model is shipped to edge devices. Further training may take place to individualise the model to the specific edge device.
Predictive text function found in mobile phones is an example of this process.
In case of IoT devices, there may be no local data available for training prior to deployment (if the device, along with ML component is deployed at the same time).
In such case, transfer learning may be used to train a model on general data, before fine-tuning on the edge for the specific local environment.
For Tensorflow models, [Tensorflow Lite](https://www.tensorflow.org/lite) can be used for Edge use cases.

**Hyperparameter optimisation**
Hyperparameter optimisation is performed to tune the structure of the model (number of trees in Random Forest, number of neurons in a layer of a NN etc).
A single run of hyperparameter optimisation is resource intensity (usually involves full training of the model).
Therefore, it is often desirable to run multiple runs at the same time.
This can be achieved using Kubernetes jobs as described above.  
Random search of hyperparameters is considered a good approach so no information transfer between individual runs is required.
Informed methods such as Tree of Parzen Estimators (TPE) are also available through Python libraries such as [hyperopt](https://github.com/hyperopt/hyperopt) which use information from previous runs to inform the selection of hyperparameters.

**Training Checkpoints**
If you have a long-running training there is an increased risk that your training may crash and all training progress is lost.
The reasons for such a crash may vary very much , e.g. running out of memory, hardware failure or coding bugs.
To make trainings more resilient checkpoint can be used.
The main idea is to save the current state of the training to file and use these checkpoints later in case of a crash. Some frameworks implement chackpoints directly,  e.g. [checkpoints in Keras](https://machinelearningmastery.com/check-point-deep-learning-models-keras/).

Remark: Some people may suggest to leverage checkpoints for generalization and tune-ability as well; We recommend against doing that:
For generalization there are dedicated APIs, therefore we recommend to use these APIs instead of checkpoints.
For tune-ability, this is theoretically possible, but is an advanced topic and highly dependent on the model at hand, therefore we recommend to only use it, if the data science team knows absolutely what they do, else it should not be applied.

### Machine Learning Frameworks
  
Frameworks should always be used for Machine Learning.
They offer a standardization and optimization with a  a huge support from the open source community.

**TensorFlow** is an Open Source Framework developed by Google, which is written with a Python API powered by a C/C++ engine. TensorFlow offers additional libraries for Java, C++, Javascript, Go and Swift with high level and low level APIs. Tensorflow 2.0 introduced **Keras** as the official high level API. TensorFlow Lite is a Framework for [Edge Deployment](./edge_deployment.md) with support for model compression.

**Keras** is a high-level neural networks library that is running on top of TensorFlow. It allows for easy and fast prototyping as well as running in deep learning.

**PyTorch** is a Open Source Framework developed by Facebook with a Python API over a engine powered by C++, Python and CUDA. It offers a low level Python API and integrates well with standard Python libraries.

### Examples

To find examples for these guidelines, go to the example repository: [MLOps pipeline](https://github.ibm.com/datascience-ibm/example-mlops-model-pipeline).

In this implementation the model is trained on Kubernetes, which is hosted on AWS EKS. 
AWS EKS is a managed service for hosting Kubernetes clusters, it helps with configuration, provisioning and scaling of the cluster.
Thus the training environment can be scaled elastically to the requirements of the input data set and the complexity of the algorithm.

To be able to train on the Kubernetes cluster, a docker container image is built, which contains the necessary data and files.

```make
docker:
	docker build . --build-arg BUILD_NUMBER=$(version) -t oncology_training
``` 

The command above triggers the build of the docker container based on the `Dockerfile` in the repository.

```docker
# the base image is python:3.7-slim containing the necessary python environment
FROM python:3.7-slim
# the build number is transferred from the caller of the command
ARG BUILD_NUMBER
# install latest updates
RUN apt-get update \
&& apt-get install make -y \
&& apt-get clean
# install pipenv
RUN pip install pipenv
# copy the working tree into the container to the /model folder
COPY ./ /model
# change the working directory to the /model folder
WORKDIR /model
# set the version environment variable to the build number
ENV VERSION=$BUILD_NUMBER
# execute the install and upload section from the makefile - this is only executed, when the container is run
CMD make install && make version=$VERSION upload
```

After the build of the container, it is pushed to the container registry of AWS (ECR).
From there it can be accessed by the Kubernetes cluster.

The configuration for the Kubernetes job can be viewed in the `job.yaml` file.
The docker container image used for this is described here, which is the image described above:

```yaml
containers:
        - name: oncology-training
          image: 294341714852.dkr.ecr.eu-central-1.amazonaws.com/ai_scale_oncology_training_trainer
          imagePullPolicy: Always
```

This image contains all the necessary code, files and environment to train the model.
The corresponding step in the makefile is:

```make
deploy: publish
	kubectl delete -f ./job.yaml --ignore-not-found
	kubectl apply -f ./job.yaml

```

When the container is run, the `CMD` section of the Dockerfile is executed.
This triggers the install and upload section of the `makefile`:

```make
install:
	pipenv install --deploy --dev
...
upload: upload.model upload.monitoring
```

The upload command includes testing, training and uploading the trained model to Nexus:

```make
upload.model: version.model package.model
	echo "[distutils]\nindex-servers =\n    nexus\n[nexus]\nrepository: ${NEXUS_LOCAL_REPO_URL}\nusername: ${NEXUS_USERNAME}\npassword: ${NEXUS_PASSWORD}" > ./.pypirc
	pipenv run twine upload --config-file .pypirc -r nexus ./dist/*
```

This section calls `version.model` and `package.model`:

```make
version.model: current_version = $(shell pipenv run python setup.py --version)
version.model: new_version = $(patsubst %.0,%.$(version),$(current_version))
version.model:
	pipenv run bumpversion --current-version $(current_version) --new-version $(new_version) --no-commit --no-tag patch setup.py
```

First the right version is set and used for the bumpversion command.

```make
package.model: doc train test
	pipenv run pipenv-setup sync
	pipenv run pipenv-setup check
	mkdir -p build
	pipenv run python setup.py egg_info -e build bdist_wheel
```

Then the model is trained and tested before it is packaged into a Python distribution wheel.
The actual training is executed in this section of the `makefile`:

```make
train: clean dvc build.training
	pipenv run python -m oncology_training -o src/oncology_model/model/model.joblib -d data -t build/train
```

For this the `build.training` section needs to be completed:

```make
package.training: clean unittest
	mkdir -p build/training_build
	pipenv run python training_setup.py egg_info -e build/training_build bdist_wheel -d build/training_build/dist

build.training: dependency-check package.training
	pipenv run pip install --no-index --find-links=build/training_build/dist/ oncology_training
```

In this `package.training` is a prerequisite, which builds a Python distribution wheel of the training code.
Afterwards it is installed here:

```make
build.training: dependency-check package.training
	pipenv run pip install --no-index --find-links=build/training_build/dist/ oncology_training
```

This concludes the rough concept of the `makefile` and training process.
