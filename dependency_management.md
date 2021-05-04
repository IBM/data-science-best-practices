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


## Chapter 8 - Dependency Management

### Why do we need to manage dependencies?
Dependency management is like your city’s sewage system. When it’s working well, it’s easy to forget that it even exists. The only time you’ll remember it is when you experience the agony induced by its failure.

Some of the goals that a healthy dependency management process tries to achieve are:
- Builds should be stable across environments. If a project builds on my machine, it should build on others’ machines and on our build server.
- Builds should be stable over time. If a project builds now, it shouldn’t break in the future.
- Any team member should be able to easily download, build, and make changes to a project.
- We should be able to have many different projects with large dependency trees without running into [dependency hell](https://en.wikipedia.org/wiki/Dependency_hell).

### What is `pip`?  
`pip` is a package manager for Python. That means it’s a tool that allows you to install and manage additional libraries and dependencies that are not distributed as part of the standard library. 

The Python installer installs `pip`, so it should be ready for you to use, unless you installed an old version of Python. You can verify that pip is available by running the following command in your console:
```
$ pip --version
pip 19.2.3 from /Users/vladzamfirescu/opt/anaconda3/lib/python3.7/site-packages/pip (python 3.7)
```

`pip` is a Python tool that specializes in installing Python packages.

For example, just run `pip install numpy` to install `numpy` **and its dependencies**. `pip` also helps you to keep your version control repositories small by giving you a reproducible way to install packages without needing to include them in your source code repo.

Not only does `pip` let you install normal source packages, but it can also install packages from source control repositories, [wheels](https://wheel.readthedocs.io/en/latest/), and legacy binary distribution formats.

### Using `requirements.txt`  
When developing Python applications today, it’s standard practice to have a `requirements.txt` file in the root of your repository.

It’s easy to get a Python project off the ground by just using `pip` to install dependent packages as you go. This works fine as long as you’re the only one working on the project, but as soon as someone else wants to run your code, they’ll need to go through the process of figuring which dependencies the project needs and installing them all by hand. Worse yet, if they install a different version of a dependency than the one you used, they could end up with some very mysterious errors.

To prevent this, you can define a `requirements.txt` file that records all of your project’s dependencies, versions included. This way, others can run `pip install -r requirements.txt` and all the project’s dependencies will be installed automatically. 

Placing this file into version control alongside the source code makes it easy for others to use and edit it. In order to ensure complete reproducibility, your `requirements.txt` file should include all of your project’s transitive (indirect) dependencies, not just your direct dependencies.

```
$ cat requirements.txt

numpy
pandas
pytest
wheel
```

This is an example of a basic `requirements.txt` file, and is effectively the user experience that everyone using requirements files wants. However, when a `requirements.txt` file like this is used to deploy to production, unexpected consequences can occur. Effectively, because versions haven’t been pinned, running `$ pip install` **will give you different results today than it will tomorrow**.

***This is bad***. As different versions of sub-dependencies are released, the result of a fresh $ pip install -r requirements.txt will result in different packages being installed, and potentially, your application failing for unknown and hidden reasons.

To avoid the pitfalls of a basic `requirements.txt` file, you can define a complete list of all dependencies a project has, each with exact package versions specified.

```
$ cat requirements.txt

numpy==1.18.1
pandas==1.0.0
pytest==5.3.5
wheel==0.32.2
```
This is considered a best practice for deploying applications, and ensures an explicit runtime environment with deterministic builds.

All dependencies, including sub-dependencies, are listed, each with an exact version specified.

This type of `requirements.txt` is generated from the output of running `$ pip freeze` from within a current working runtime environment for the application. This encourages dev/prod parity, and encourages you to treat code within external packages with the same level of respect as your application code (because it is your application code).

Even though the fixed-version format for `requirements.txt` is considered to be a best practice, it can sometimes be a bit cumbersome. Namely, if you are working on the codebase of your proejct, and want to `$ pip install --upgrade` some/all of the packages, you wouldn't be able to do so easily. 

For additional information on the `requirements.txt` file, feel free to consult the [pip user guide](https://pip.pypa.io/en/latest/user_guide/#requirements-files).

### Taking dependency management to the next level with `pipenv` or `poetry`

> **Q: Is there a better approach to ```pip freeze```?**<br/>
> A: Yes, because there is a high risk to include dependencies that you don't actually need when executing `pip freeze`. You should always aim to minimize your dependencies.

`poetry` is a dependency manager for Python projects and improves on the more traditional `requirements.txt` method described previously.
If you’re familiar with Node.js’ `npm` or Ruby’s `bundler`, it is similar in spirit to those tools.
While `pip` can install Python packages, `poetry` is recommended as it’s a higher-level tool that simplifies dependency management for common use cases.

It automatically creates and manages a `virtualenv` for your projects, as well as adds/removes packages from your `pyproject.toml` as you install/uninstall packages. It also generates the ever-important `poetry.lock`, which is used to produce deterministic builds.

`pipenv` is doing the same thing; the respective files are a `Pipfile` and a `Pipfile.lock`.

> The key differentiation between `pipenv` and `poetry` is that `poetry` includes also packaging and distribution for your own Python package, which `pipenv` doesn't.

Poetry tends to be a bit more modern and quicker, but both work well and should always be preferred over plain `pip`.

> One very cool thing about pipenv is that if you already have a `requirements.txt` file in your project structure it will automatically identify it when you run `pipenv install` and convert your `requirements.txt` into a `Pipfile` and generate the `Pipfile.lock` as well.

- [`poetry` Website](https://python-poetry.org/)
- [`pipenv` Website](https://pipenv.kennethreitz.org/en/latest/)

#### Usage

Example `pyproject.toml` (for `poetry`)

```toml
[tool.poetry]
name = "poetry-demo"
version = "0.1.0"
description = ""
authors = ["Sébastien Eustace <sebastien@eustace.io>"]

[tool.poetry.dependencies]
python = "*"

[tool.poetry.dev-dependencies]
pytest = "^3.4"
```

Example `Pipfile` (for `pipenv`)

```toml
[[source]]
url = "https://pypi.python.org/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = "*"


[dev-packages]
pytest = "*"
```

> **Note:**
> The lock files (`poetry.lock` and `Pipfile.lock`) are technically regular, editable files.
> However, do not adjust them manually. Always let `poetry` or `pipenv` adjust them.

General Recommendations & Version Control (poetry)

- Generally, keep both `pyproject.toml`, `poetry.lock` (and `poetry.toml`) in version control.
- Specify your target Python version in your `pyproject.toml`’s `[tool.poetry.dependencies]` section. Ideally, you should only have one target Python version, as this is a deployment tool.
- Use `poetry add <module>` to add additional packages ([more details](https://python-poetry.org/docs/cli/#add)).
- Use `poetry install --no-root` in order to just install the dependencies (and without installing your own module) - [more details](https://python-poetry.org/docs/cli/#install).
- Note that the `pyproject.toml` uses the [TOML Spec](https://github.com/toml-lang/toml#user-content-spec).
- Full `poetry` CLI documentation can be found [here](https://python-poetry.org/docs/cli/).

General Recommendations & Version Control (pipenv)

- Generally, keep both `Pipfile` and `Pipfile.lock` in version control.
- Do not keep `Pipfile.lock` in version control if multiple versions of Python are being targeted.
- Specify your target Python version in your `Pipfile`’s `[requires]` section. Ideally, you should only have one target Python version, as this is a deployment tool. `python_version` should be in the format `X.Y` and `python_full_version` should be in `X.Y.Z` format.
- `pipenv install` is fully compatible with `pip install` syntax, for which the full documentation can be found [here](https://pip.pypa.io/en/stable/user_guide/#installing-packages).
- Note that the `Pipfile` uses the [TOML Spec](https://github.com/toml-lang/toml#user-content-spec).
- Full `pienv` CLI documentation can be found [here](https://pipenv.kennethreitz.org/en/latest/cli/).

### Organisation-wide Open Source Software (OSS) Guidelines

#### :warning: IMPORTANT :warning: Please note that OSS usage (both internal or external) including consumption and contribution must comply with licensing, copywright law and your organization's internal guidelines.
Copyright law gives authors rights, such as:
  * The author determines appropriate uses of a work.
  * You cannot reproduce or modify a work without an author's permission.
  * The author may grant permission via a license, for example, an open source license.
  	* A license grants permission, but may also impose obligations.
  
  #### _It is mandatory for IBMers to adhere to the [IBM OSS Guidelines and associated processes](https://github.ibm.com/datascience-ibm/data-science-best-practices/blob/master/ibm_oss_guideline.md) (IBM Internal)_.

#### MIT
A short, permissive software license. Basically, you can do whatever you want as long as you include the original copyright and license notice in any copy of the software/source.  There are many variations of this license in use.

[MIT License Full Text](https://opensource.org/licenses/MIT)

#### Apache 2.0
You can do what you like with the software, as long as you include the required notices. This permissive license contains a patent license from the contributors of the code.

[Apache 2.0 License Full Text](https://opensource.org/licenses/Apache-2.0)

#### BSD
The BSD 2-Clause and BSD 3-Clause licenses allows you almost unlimited freedom with the software so long as you include the BSD copyright notice in it (found in full text reference below).

[BSD-2-Clause Full Text](https://opensource.org/licenses/BSD-2-Clause)  
[BSD-3-Clause Full Text](https://opensource.org/licenses/BSD-3-Clause)

#### GPL
You may copy, distribute and modify the software as long as you track changes/dates in source files. Any modifications to or software including (via compiler) GPL-licensed code must also be made available under the GPL along with build & install instructions.

[GPL 2.0 Full Text](https://opensource.org/licenses/GPL-2.0)  
[GPL 3.0 Full Text](https://opensource.org/licenses/GPL-3.0)

#### LGPL
This license is mainly applied to libraries. You may copy, distribute and modify the software provided that modifications are described and licensed for free under LGPL. Derivatives works (including modifications or anything statically linked to the library) can only be redistributed under LGPL, but applications that use the library don't have to be.

[LGPL 2.0 Full Text](https://opensource.org/licenses/LGPL-2.0)  
[LGPL 2.1 Full Text](https://opensource.org/licenses/LGPL-2.1)  
[LGPL 3.0 Full Text](https://opensource.org/licenses/LGPL-3.0)

### Examples

To find examples for these guidelines, go to the example repository: [MLOps pipeline](https://github.ibm.com/datascience-ibm/example-mlops-model-pipeline).

In this example implementation `pipenv` is used. 
As described above the two relevant files are `Pipfile` and `Pipfile.lock`. Both of these are located in the root folder of the example implementation.

`Pipfile:`
```
...

mypy = "==0.761"
pandas = "==1.0.0"
pipenv = "==2018.11.26"
pipenv-setup = "==3.0.1"
sphinx = "==2.3.1"
twine = "==3.1.1"
jeepney = {version = "*", sys_platform = "== 'linux'"}
secretstorage = {version = "*", sys_platform = "== 'linux'"}
wheel = "==0.34.2"
yapf = "==0.29.0"

...
```

In the example above you can see the different groups for packages: `dev-packages` and `packages`.
The first set is only installed for the development environment, which includes building and experiments.
The second set is used for the actual final package after being assembled and is therefore a lot smaller.
One specialty in the `dev-packages` section are the linux specific packages, these are only required in a linux environment.
This is added to offer cross-platform compatibility.
`Pipenv` evaluates the current environment and then installs packages based on the result.

`Pipfile.lock`

```

    "_meta": {
        "hash": {
            "sha256": "3000b68e11f41330aa5a7500462b47e913e95ae067eca401dcd70a62a204bbc6"
        },
        "pipfile-spec": 6,
        "requires": {
            "python_version": "3.7"
        },
        "sources": [
            {
                "name": "pypi",
                "url": "https://pypi.python.org/simple",
                "verify_ssl": true
            },
            {
                "name": "nexus",
                "url": "http://$NEXUS_USERNAME:'${NEXUS_PASSWORD}'@a737dee0740fd11eaa014026127b7ccb-1361029674.eu-central-1.elb.amazonaws.com:8081/repository/pypi-group/simple",
                "verify_ssl": false
            }
        ]
    },

       
       ...
```

As described above, this file contains the specific versions of the required packages and the corresponding hash.
This dependency tree is built recursively and satisfies all transitive dependencies. 
In the `makefile` the pip environment is created with the specific `pipenv` command:
```
...

install:
	pipenv install --deploy --dev

...
```
This installs both dev and default packages and aborts if the `Pipfile.lock` is out of date.
