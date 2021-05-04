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
- [Chapter 21 - Usage Recommendations (Scalability)](./recommendation.md#chapter-21---usage-recommendations)
- [License & Contributing](./license.md)


## Chapter 14 - Cloud-Deployment

### Docker

#### What is Docker?
A docker container can be described like a usual shipment container. It packages different items together into one unit.
This unit is standardized, so it can be deployed to standardized environments, which are able to host the container.
The base of every docker container is an lightweight operating system, which is installed during runtime. 
In addition to that there is a cookbook for how to assemble the container (dockerfile). So the docker host knows how to
build and execute the container. 
In this dockerfile all the necessary commands to build the container are located. 
Often there is code executable copied to the container so it can be executed during runtime.
With bringing its own operating system, data, files and runtime environment a docker container is able to run on most
machines without further configuration. The only prerequisite is a running docker host. 
All cloud provider offer a managed environment to run docker containers and also scale automatically if the workload 
requires to do so.
So it is very easy to port workloads from one runtime host to another one without caring about infrastructure.
This avoids the classic *"But it worked on my machine"* problem.

Typically when you build a docker container the output is a docker container image. These images are the units, that
can be shipped or deployed to a running docker host for execution. 
These images are usually stored in a so-called container registry. There are multiple flavours of a registry: Nexus is 
one example for an open source distribution, but most of the cloud providers also maintain their own container registry.
When an image is built on a build server it is pushed to a container registry after the job has finished. The registry
keeps track of images and handles versioning seamlessly for the user. 
More information on docker can be found [here](https://docs.docker.com/get-started/)

- For Nexus guide: https://support.sonatype.com/hc/en-us/sections/203012608-Getting-Started


#### How to use Docker
Firstly, Docker needs to be installed.
- [Mac](https://docs.docker.com/docker-for-mac/install/)
- [Windows](https://docs.docker.com/docker-for-windows/install/)

To check that the installation was successful, a HelloWorld example can be run.
```commandline
docker run hello-world
```
This should generate a message outlining the steps taken by docker.
This is one example of how a pre-existing docker image can be used. 
A number of images can be found in [Docker Hub](https://hub.docker.com/search?q=&type=image).

Custom images can also be build using dockerfile. 
Typically, a base image is used to take care of basics. 
Base images can be found in [Docker Hub](https://hub.docker.com/search?category=base&source=verified&type=image)
In the example below Python 3 base image is used to create our image. In this particular image, pip comes pre-installed, 
so pip command can be used to install all required Python module (Otherwise, pip itself has to be installed first).
```
# uses Python 3.7 base image 
FROM python:3.7

# installs all required python packages such as pipenv
RUN pip install pipenv
```
`COPY` command can be used to move files from local machine into the docker image.

This dockerfile can then be used to build an image. 
If the dockerfile is called `Dockerfile`, there is no need to specify filename.

```commandline
docker build -t my-python-app .
```
*(See more build options [here](https://docs.docker.com/engine/reference/commandline/build/))* <br>

To run a container based on the created image, run the following
```commandline
docker run my-python-app
```
*(See run options [here](https://docs.docker.com/engine/reference/commandline/run/))* <br>

#### Keeping secrets
In many cases, some form of authentication needs to be performed when building an image; 
for example, cloning the latest version of the source code from a repo into the docker image.
If login details were passed as part of a command or a copied in a file, these would be retrievable from the container.
This is obviously undesirable. To avoid this [multi-stage deployment](https://docs.docker.com/develop/develop-images/multistage-build/) can be used.

The idea behind this is to create a temporary container with credentials to access the source code repository, 
which is then used to copy the content into the container that will be distributed.
The basic dockerfile structure is as follows:
```
FROM python:3.7 as builder

ARG USERNAME
ARG PASSWORD

# do thing with USERNAME and PASSWORD

FROM python:3.7

# do things

COPY --from=builder <copy from builder>
```

#### Container Repository
Similarly to source code repository, docker images can also be stored in a repository.
Dockerhub is one such repository; however, each of the Cloud Platforms ([AWS](https://aws.amazon.com/ecr/),
 [Azure](https://azure.microsoft.com/en-in/services/container-registry/), 
 [IBM Cloud](https://www.ibm.com/uk-en/cloud/container-registry), 
 [GCP](https://cloud.google.com/container-registry)) also provide repository services.
 
**To push an image to a repository:** <br>
Tag the image with repository user and the name of the repository
 ```commandline 
docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]
```
Push it to the repository
```commandline
docker push <hub-user>/<repo-name>:<tag>
```
*([See more](https://docs.docker.com/docker-hub/repos/#pushing-a-docker-container-image-to-docker-hub))* <br>
Similar process is done for the Cloud Platform container registries 
(e.g. hub-user for AWS is of the format: `<aws_account_id>.dkr.ecr.<region>.amazonaws.com`).

### Kubernetes Intro

Kubernetes is an open source software solution to run, scale and deploy container workloads. It is built to run containers
on a distributed environment. Kubernetes was originally developed by Google and is aimed for enterprise scenarios.
The smallest unit in Kubernetes is a Pod, which may hold one or many containers. A node holds multiple pods and orchestrates 
them.
Docker and other container solutions are supported.

#### Runtime Deployment

Often ML models will be deployed as part of a larger micro service, which offers a [REST-API](https://www.ibm.com/cloud/learn/rest-apis) to its users for consumption.
Therefore, they run in an always-on fashion waiting for incoming requests form the client.
A typical deployment is therefore to use a [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/) to ensure there is a stable connection point for clients to connect to your web / micro service.

For the REST-API it is recommended to provide the some documentation.
The most common way is using the [OpenAPI specification](https://openapis.org) (sometimes also referred to as [Swagger](https://swagger.io/)).
It is recommended to apply the same / similar design principles from the [source code](https://github.ibm.com/datascience-ibm/data-science-best-practices/blob/master/source_code.md#coding-design-guideline) also to APIs.
For more detailed API design guidelines, you may want to refer to the [API Stylebook](http://apistylebook.com).

#### Training Deployment

The training of a ML model is usually a process with a clear start and a clear end.
This process may be run multiple time (for multiple iterations) and/or sometimes in parallel, but the statement above holds true.
Because of this clear start and end, it is recommended to not use a Kubernetes Service but instead use a [Kubernetes Job](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)

### Managed Services

Most cloud providers offer managed AI Services: [Watson Studio](https://www.ibm.com/uk-en/cloud/watson-studio) (IBM Cloud), 
[Sagemaker](https://aws.amazon.com/sagemaker/) (AWS), [Azure ML](https://azure.microsoft.com/en-gb/services/machine-learning/) (Microsoft) etc.
These managed services offer a variety of different functionality. Primarily most offer an environment to host Jupyter 
notebooks. Another feature for most of the services is to train and host machine learning models. Therefore it is 
possible to launch model trainings by using the offered API. Mostly docker containers are used to package the training
code and the necessary code environment. These containers can then be scheduled on the cloud providers' compute resources
by specifying machine types (GPU-, Compute- or Memory-optimized) and sizes. In addition a similar API is offered to host
trained machine learning models - these models can be of various types. Some tools also offer monitoring solutions to
keep track of the most important KPIs of the models. Generally these managed services offer a very convenient way to 
ramp up small machine learning engagements quickly without the need to administer any resources.

### Example

To find examples for these guidelines, go to the example repository: [MLOps pipeline](https://github.ibm.com/datascience-ibm/example-mlops-model-pipeline).

In this implementation AWS is used as a cloud provider. 
The services used here are:
 - AWS S3 - to host the training data, model and testing KPIs and as remote storage for Nexus
 - AWS EKS - to provision a managed Kubernetes Cluster with two EC2 nodes for training the model, hosting Nexus and running the web-service
 - AWS QuickSight - to visualize the generated data in a simple reporting tool, which can directly access AWS S3 as data source
 - AWS ECR - to version the generated container images and make them available for AWS EKS

The interaction with the cloud platform is primarily triggered via the makefile and the AWS CLI.
The AWS CLI offers the interaction with most AWS services via CLI commands. 
```
upload.monitoring: test
	pipenv run aws s3 cp ./build/unittest-coverage.json s3://machine-learning-metrics/trainer/unittest-coverage/unittest-coverage-$(version).json
	pipenv run aws s3 cp ./build/test-coverage.json s3://machine-learning-metrics/trainer/test-coverage/test-coverage-$(version).jsone
```
In the example above the AWS S3 interface is used to upload the generated test coverage files to a bucket.
For working with the EKS cluster, the `kubectl` command can be used, as soon as the EKS cluster is defined as target.
For deploying the web-service (that hosts the model for prediction), this is scripted here:
```
deploy: publish
	kubectl set image deployments -l app=oncology-webservice,team=$(team_label) oncology-webservice=$(tag):$(version)
```
Refer to the [web service repository](https://github.ibm.com/datascience-ibm/example-mlops-web-service) for the respective `makefile`.
The web-service is used for offering the trained model via a REST API to the consumer.
For this purpose, the web-service is provisioned on the Kubernetes cluster.
The REST endpoints are offered by using the Python Flask library:
```python
def post(self) -> Prediction:
        """
        The Breast Cancer analysis API predicts based on the given
        measurements if a tumor is harmless (benign) or evil (malignant).
        """

        # parse input json (body)
        input = request.json

        # let model predict class for patient
        return self.service.predict(Tumor(**input))
```
The post endpoint is specified in the example above.
So when a post operation is called from a web client, the post method is called within the web-service.
In the post method, the predict method from the classifier of the model is called.
```python
from oncology_model.datamodel import Tumor, Prediction
from oncology_model.classifier import Classifier


class Service:

    def __init__(self) -> None:
        self.classifier = Classifier()

    def predict(self, tumor: Tumor) -> Prediction:
        return self.classifier.predict(tumor)
 ```
The service class imports the distributed `oncology_model` package (refer to the examples section of [Chapter 13 - Distribution (Deploy)](./distribution.md#chapter-13---distribution)).
The classifier object is used to call its predict function. 
The input for the prediction is the tumor object, which is defined in the data model of the `oncology_model` package.
As seen in the post method, the tumor is parsed from the incoming JSON document from the API call.
The return value of the prediction is also defined in the data model of the `oncology_model`:
```python
class Prediction(Enum):
    BENIGN = "benign"
    MALIGNANT = "malignant"

    @classmethod
    def from_class(class_, id: int) -> 'Prediction':
        switch = {2: Prediction.BENIGN, 4: Prediction.MALIGNANT}
        value = switch.get(id)
        if not isinstance(value, Prediction):
            raise ValueError(f"'id' seems to be an invalide class id, was:{id}")
        return value

    @classmethod
    def from_classifier(class_, value: int) -> 'Prediction':
        switch = {0: Prediction.BENIGN, 1: Prediction.MALIGNANT}
        prediction = switch.get(value)
        if not isinstance(prediction, Prediction):
            raise ValueError(
                f"'value' seems to be an invalide clasifier result value, was:{value}"
            )
        return prediction
```
There are only two possibilities: Either "benign" or "malignant".
