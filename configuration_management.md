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

## Chapter 9 - Configuration Management

When developing code, certain parameters need to be defined with a non-arbitrary value.
While optimization of an application these are usually then tweaked and adjusted for the specific  need.
Here are 5 ways to handle such configuration parameters:

### Hard-Coded Values

```python
...
input, target = split(test_ratio=0.2)
...
````

Hard coding a value to a parameter is the most simple, but also the most error prone way to manage configuration.
It can be used for constant values that are inherently tied to the specific use case: E.g. in the case of a image classification model a parameter defining the input as either image or audio can be hard coded to image.
However hard coding **must not be used**, if one of the following cases applies:

- The value is secret (e.g. a password).
- The value needs to be set consistently at multiple (more than one) places.
- There is a slightest of chance that the value may needs to be changed in the future.

### Constants

```python
TEST_RATIO = 0.2
...
input, target = split(test_ratio=TEST_RATIO)
...
```

Constants are still defined / hard coded in the source code. The key difference is that constants are defined in a central place, either at the top / beginning of your *.py file (Local Constant) or in a dedicated constant Python module (Global Constant).
It is recommended to use local constants whenever the constant's value is only needed in the current Python module; if the constant is used in two or more Python module then consider using a global constant.

> **Caution:** Python doesn't have a hard-enforced visibility concept (e.g. `private` and `public`). From Python perspective the constant can be changed  like every other variable. Therefore restrict yourself from doing that!

Constants are the preferred approach for configuration that is hardly ever changed. **However it must not be used for secrets!**

### Configuration File

```json
{
...
	"test_fraction": 0.2,
...
}

```

```python
...
config = json.loads(pkgutil.get_data(__name__, "../config/" + file).decode("utf-8"))
...
input, target = split(test_ratio=config["test_fraction"])
...
```

For more volatile configuration a configuration file is often handy.
This is especially true for configuration that is done by non-developers, like the operations team.
Therefore it is recommended to use a non coding format, e.g. `.json` or `.env` files.
**Configuration files can be used to handle secrets, however it must ensured to be  not committed to git.**

In general, configuration files are very common in software engineering practice, but in recent history there usage decreases a bit.

### Environment Variable

```bash
export TEST_FRACTION=0.2
```

```python
...
TEST_FRACTION = os.getenv('TEST_FRACTION')
...
input, target = split(test_ratio=TEST_FRACTION)
...
```

For volatile configuration, environment variables are usually a good option.
Since environment variables are managed on OS (operating system) level, they are global and naming conflicts must be avoided.

- [Python - Command line and environment](https://docs.python.org/3/using/cmdline.html)
- [Medium - Hiding secret info in Python using environment variables](https://medium.com/dataseries/hiding-secret-info-in-python-using-environment-variables-a2bab182eea)

### Secret Management

When **secret vaults** are accessible, they should be used to store and access credentials in a project. An example for a secret vault is [HashiCorp Vault](https://www.vaultproject.io/).
In addition, many applications provide built in secret vaults that can be used,

### CLI Parameters

```bash
python -m <module name> --test-fraction 0.2
```

```python
import getopt
...
opts, args = getopt.getopt(argv, "t:",["test-fraction="])
for opt, arg in opts:
    if opt in ("-t", "--test-fraction"):
        test_fraction = int(arg)
...
input, target = split(test_ratio=test_fraction)
...
```

For configuration is highly volatile and has no default a CLI parameter is sometimes a good option. **CLI parameters are not a good option for secrets**, since shell commands are often logged.

- [Python - Command line and environment](https://docs.python.org/3/using/cmdline.html)
- [Tutorial: Python - Command Line Arguments](https://www.tutorialspoint.com/python/python_command_line_arguments.htm)

### Configuration Management in distributed environments

Distributed environments that are larger in size require the management of configurations in multiple instances.

 [Apache Zookeper](https://zookeeper.apache.org/) is a open-source server which enables distributed coordination.
It is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. All of these kinds of services are used in some form or another by distributed applications. 
