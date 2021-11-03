---
layout: default
title: Documentation
nav_order: 5
---
# IBM Data Science - Best Practices

## Documentation

In general source code is read more often then written.
This is why source code should be coded in a clear and readable way, so it becomes somewhat self-explaining (see [Source Code chapter](./source_code.md#chapter-4---source-code)).
However even with the most clear naming and coding additional documentation is needed.
Documentation provides a more detailed explanation of what certain code is intended to do.
It also covers the expected inputs and the expected outputs.
It describes the edge cases and exceptions (what happens if something not anticipated happens).

### Source Code Documentation

In Python this documentation is done via `Docstrings` (see [PEP-257](https://www.python.org/dev/peps/pep-0257/)).

```python
def complex(real=0.0, imag=0.0):
    """Form a complex number.

    Keyword arguments:
    real -- the real part (default 0.0)
    imag -- the imaginary part (default 0.0)
    """
    if imag == 0.0 and real == 0.0:
        return complex_zero
    ...
```

Things to cover in a `Docstring` in a `function` or `method`:

- Brief description of the functionality.
- Description of each  argument / parameter / input , including their type and default value.
- Description of each output / return value, including their type.
- Description of every (realistic) error and exception, including their reason.

Things to cover in a `Docstring`in a `class`:

- Brief description of the class itself.
- `Docstring` for constructor method (see `Docstring` for `method`).
- `Docstring` for each method (see `Docstring` for `method`).

#### Automatic Documentation Generation

The `Docstring` documentation is easy and simple to generate for the developer but for consumers / users of an API to navigate through the source code to find the documentation is cumbersome. Therefore we recommend the usage of tools like [Sphinx](http://www.sphinx-doc.org/en/master/index.html) to automatically generate HTML-based documentation.

- [Sphinx - Getting Started](http://www.sphinx-doc.org/en/master/usage/quickstart.html)

### Source Code Comments

The `Docstring` documentation is mostly aimed at the user of an API.
However sometimes it might be useful to explain how and why some functionality was implemented in a certain way.
This documentation is mostly done via comments inside the method and is the preferred way to document information that is relevant for fellow developers working on the same method but not for the consumers / users of the method.

```python
def train():
    ...
    # random initialization of parameters improves training performance
    parameters = np.random() 
    ...
```

Comments should be brief but also meaningful.
If a concept is to complex to be explained in a few sentences with source code comments, then maybe an alternative documentation document might be useful. This document could than be referenced in a source code comment.

### Repository / Component Documentation

In a project there are many different levels of documentation, e.g. for [architecture](./architecture.md#chapter-3---architecture) or source code (see above).
For each source code component there should be documentation on the purpose of that component and how to use it.

This is usually down in the ReadMe file (often `readme.md`).

```markdown
# <project name>

<here goes the business backgroud - why?>
<here goes purpose - what?>
<here goes brief description of project  - how?>

## How to use

<here goes installation instruction>
<here goes simple usage scenario>

## License

<here goes link to license>

## Contact

<here goes contact information>

```

Many times one source code repository (see [Versioning chapter](./versioning.md#chapter-6---versioning)) contains exactly one component so here there is only the need for just one ReadMe file. 

If there are more than one component in one source code repository then one ReadMe file for each component is required plus an additional ReadMe file for the repository. The repository ReadMe file basically needs to explain the grouping of the contained component and link to each components individual ReadMe file.

### Example

To find examples for documentation refer to: [MLOps pipeline](https://github.ibm.com/datascience-ibm/example-mlops-model-pipeline).

In the root folder you find the `readme.md` file, which states the purpose of this repository and some general information on the content.

## AI FactSheets 360

AI FactSheets is a project to enable trust in AI by increasing transparency and enabling governance. This provides information to better understand, how an AI model or service was created. The consumer of the model can therefor determine if it is appropriate for their situation. AI Governance enables to specify and enforce policies how an AI model should be constructed or deployed.

A FactSheet is a collection of relevant information about the creation and deployment of an AI model or service. It documents key Facts about how an AI model or service was developed, tested, deployed, monitored and modified. FactSheets get modeled after a supplier´s declaration of conformity (SDoC). They are tailored to the AI model or service that is documented and to the target audience or consumer. FactSheets capture facts from the entire AI lifecycle.

To find more about AI FactSheets 360 refer to [IBM Research AI FactSheets 360](https://aifs360.mybluemix.net/).

### Recommended Steps for creating Fact Sheets

1. Know your FactSheet consumers
2. Know your FactSheet producers
3. Create a FactSheet template
4. Fill In FactSheet Template
5. Have actual producers create a FactSheet
6. Evaluate actual FactSheet with consumers
7. Devise other templates and forms for other audiences and purposes

[Methodology for Creating AI FactSheets](https://aifs360.mybluemix.net/methodology)

#### Examples

For FactSheet examples about an Audio Classifier, Object Detector, Image Caption Generator, Text Sentiment Classifier and Weather Forecaster refer to [FactSheets Examples](https://aifs360.mybluemix.net/examples).
