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

## Chapter 4 - Source Code

Most of the day-to-day work of a data scientist is writing code.
For data scientists at IBM this usually means Python 3 source code.
In general source code is read much more often then it is written - since it is written just once. Therefore it is important to optimize the source code to be well readable / consumable.

### Python Style Guide

The very basics of source code writing is the style and format. The styling and formatting for Python 3 is defined in [PEP-8](https://www.python.org/dev/peps/pep-0008/). This gives the source code a certain order and starts making code readable. An extension of PEP-8 is the [Google Python Style Guide](http://google.github.io/styleguide/pyguide.html). We recommend using the Google inspired  rather than pure PEP-8, since it is a bit more cleaner.

To apply the PEP-8 standard to source code it is recommended to use code linting. Here are a few Python libraries for linting:

- [yapf](https://pypi.org/project/yapf/0.29.0/)
- [pylint](https://pypi.org/project/pylint/)
- [pep8](https://pypi.org/project/pep8/)

These libraries can be used via the command-line to check source code for code style violations. It is recommended to code style checking [automatically](./automation.md#chapter-17---automation) on each new [code commit](./versioning.md#chapter-6---versioning) to ensure a proper and consistent code style is maintained throughout the project across the team.

> We recommend using `yapf` since it additionally offers an automatic reformatter, so you don't need to fix your formatting issues manually.

Visual Code also supports [Python linting](https://code.visualstudio.com/docs/python/linting) directly in the IDE.

### Coding Design Guideline

Like in the English language there are many grammatically written sentence, but some are cumbersome to read and others not. The exact same applies also to any programming language in general including Python.

Python defines some high-level guiding principles known as [The Zen of Python (PEP-20)](https://www.python.org/dev/peps/pep-0020/). However, these guidelines are very abstract; to make them  more tangible, we recommend taking some inspiration for [Swift programming language](https://swift.org/) developed by Apple.

- [Learn more about Interfaces](./interfaces.md)
- [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- [Swift API Design Guidelines at WWDC2016](https://developer.apple.com/videos/play/wwdc2016/403/) (The first 20 min are about API design.)

#### Guiding principles

- **Clarity at the point of use** is your most important goal. Entities such as methods and properties are declared only once but used repeatedly. Design APIs to make those uses clear and concise. When evaluating a design, reading a declaration is seldom sufficient; always examine a use case to make sure it looks clear in context. [from Swift]

```python
divide(4, by=2) #reads well: divide 4 by 2
todos.insert(new_todo, at=0)
todos.insert(new_todo, at_position=0)
```

- **Clarity is more important than brevity.** There is no prize for the shortest code. Make sure your code reads like an English sentence. Every word needs to add value fro the reader, but don't shorten it too much to create ambiguity.

```python
# actual API
todos.insert(0, new_todo) # doesn't read well
todos.insert(index=0, item=new_todo) # still doesn't read well

# ideally the API would look like this
todos.insert(new_todo, at=0) # optimal
todos.insert(new_todo, at_position=0) # optimal

# very bad versions of this API
T.i(t,0) # too short
todos.insert_item_at_index(index=0, item=new_todo) # too long
```

> **Caution: Python ≠ Swift**<br />
> Unlike Python, Swift is a *strongly typed language*. Meaning it enforces the proper use of types for variables, parameters, etc. at compile time. Python on the other side does not do that. Therefore, you may need to be more explicit with your naming in Python, since there is no compiler helping you deduct the types in Python.

### Naming Guidelines

Based on the previous chapters naming variables and parameters becomes relatively simple.
Depending on the thing you intend name, [PEP-8](https://www.python.org/dev/peps/pep-0008/) defines the style for this name, e.g. lower case with underscores (e.g. for method names) or capitalized words (e.g. for class names).
Based on the design guidelines, you choose a meaningful name explaining the concept behind the thing you intend to name.
The goal is to form easily readable and therefore easy to understand code.
Here are some additional points to look out for:

- Method usually do something therefore they are typically named with verbs e.g. *remove*, *insert*, *train*, etc.
- Classes, variables and parameters usually describe or contain objects. Therefore, they are typically named with nouns, like *todos*, *number*, *element*, *matrix*, etc.
- Use for collections the plural. E.g. *todo* describes a single element, whereas *todos* describe a collection of elements.
- Restrict from using single (or few) letter variables!
- Avoid abbreviations unless they are general knowledge!

> **Hint:** If you find yourself giving your variables endless names because of ambiguity, then you may want to consider restructuring your code. If you're having trouble finding a good, precise name for something, maybe you combined multiple things. In this case you may want to consider splitting it up and naming the parts then properly.

### Project Structure (Example)

```txt
PROJECT_HOME
 |-- .dvc                 <-- DVC configuration folder
    |-- config            <-- DVC configuration file
 |-- build                <-- any artifacts created during build (e.g. intermediate results) [not stored in git]
 |-- data                 <-- raw data folder [not stored in git]
    |-- *.dvc             <-- DVC metadata files
    |-- .dockerignore     <-- Files / folders to be ignored for Docker build
    |-- .gitignore        <-- Git configuration (which files shall be ignored)
 |-- dist                 <-- serialized models, packeged libs
 |-- notebook             <-- Notebooks used for exploration
    |-- *.ipynb           <-- Jupyter notebook file
 |-- src                  <-- Python source code for runtime
    |-- <py-lib>          <-- Python library
        |-- config        <-- Configuration files of library
           |-- *.json     <-- JSON config file
        |-- <sub-package> <-- Sub-package within library
           |-- *.py       <-- Python source code
        |-- *.py          <-- Python source code
        |-- py.typed      <-- Marker file for python typing (PEP-561)
 |-- tests                <-- Python testing code
    |-- test_*.py         <-- Python unit test file
    |-- systest_*.py      <-- Python system or integration test file
 |-- training             <-- Python source code for training
    |-- <py-lib>          <-- Python library
        |-- config        <-- Configuration files of library
           |-- *.json     <-- JSON config file
        |-- <sub-package> <-- Sub-package within library
           |-- *.py       <-- Python source code
        |-- *.py          <-- Python source code
 .dockerignore            <-- Files / folders to be ignored for Docker build
 .gitignore               <-- Git configuration (which files shall be ignored)
 .travis.yml              <-- Travis CI configuration (see Automation or CI/DC)
 Dockerfile               <-- Docker packaging configuration
 makefile                 <-- Build configuration (uses GNU make)
 poetry.lock              <-- Automatically expanded dependency configuration (do not edit manually) [stored in git]
 poetry.toml              <-- Poetry configuration file
 pyproject.toml           <-- Python dependency configuration and packaging configuration for runtime code (via Poetry)
 README.md                <-- Base documentation for project
 training_setup.py        <-- Packaging configuration for training code
 *.yaml                   <-- Kubernetes deployment configuration

```

### Method and Function Design

A method or a function takes some inputs, does something and produces and then produces some outputs.
It is basically a wrapping / capsulation of functionality.
The difference between a method and a function is mainly that a method belongs to a class (see Object-Oriented Programming), whereas a function is independent (see Functional Programming).

A good practice for functions is to use  them for code simplification by encapsulation and splitting the complexity of the desired overall functionality:

```python
# sample function
def request_loan(of: int, for_: CustomerId)
    amount = of
    customer_id = for_
    ...
    customer: Customer = customer_system.customer(with_id=customer_id)
    risk_system.validate_risk(of=customer)
    ...

```

Imagine all these custom methods weren't there - every small detailed action would need to be added to parent method.
This would lead to an endlessly long *main* method that is un-maintainable!
However turning every small statement into its own method would lead to a lot of useless boilerplate code.
The sweet spot lies somewhere between these two extremes.
Here are some tips for finding this sweet spot:

- A method should do exactly one thing!
- A method should provide a meaningful / useful output.
- A method should accept meaningful / realistic input.
- The functionality of the method should be easily described (one simple sentence).
- The functionality  of the method should code just once and then reuse through out the project (a.k.a. no code duplication).
- A method should be more complex then a one-liner.
- A method should not be longer than one page. (A shorter and simpler method is much easier to [properly test](./testing.md#chapter-10---testing).)

### Class Design

Since Python is also a Object-Oriented Programming (OOP) language, it offers the concept of *objects* and *classes*: An *object* has two characteristics: - attributes and behavior. The *attributes* (sometimes also *fields*) are representing the state of the *object*. The behavior represented by the *methods* implemented by an *object*.
A *class* is a blueprint for different *objects* of the same kind. *Classes* are something also referred to as *types*. *Objects* are sometimes referred to as *class instances* or just *instances*.

Example: `Human` is our class. It defines that all humans have a `hair_color` and an `eye_color` - these are the attributes.  It also defines that all humans can `smile` - the behavior specified in method(s). An instance of this `Human` class could be a `girl` object with brown `hair_color` and `eye_color`

```python
class Human:
    def __init__(self, hair_color, eye_color):
        self.hair_color = hair_color
        self.eye_color = eye_color

    def smile(self):
        ...

girl = Human(hair_color='brown', eye_color='brown')
girl.smile()
```

![girl smiling](https://media.giphy.com/media/B0vFTrb0ZGDf2/giphy.gif)

A class / objects enables programmers to meaningful bundle data structures and functions.
This is one of the most common ways in modern programming to organize the source code. Here are some tips to avoid cumbersome classes:

- Keep classes simple - reality is usually much more complex then your classes.
- Don't group random / unrelated functionality into your class.
- For functionality unrelated to any object or class, use functions instead (see Functional Programming).
- Avoid polymorphism (extensions, inheritance, etc. )!
- Avoid method overloading (same method name + different parameters)!
- Every class has their own .py file.

### Type Checking

As stated before Python is a weak typed programming language.
This means Python won't enforce type checking before execution.

This feature is appreciated by many Python developers because you don't need to create explicit interfaces, which are often considered as boilerplate. The downside is that Python is unable to warn the programmer from some stupid mistakes:

```python
# bad example
number = 'string'
...
a lot of other code here
...
print(number / 4)
```

In the example above you do net get any error from Python upfront; only if you run this you will get a *TypeError*. In strongly typed programming languages like Java, C++, Objective-C or Swift the compiling would fail already during build time because of this type mismatch.

To mitigate this issue, optional typing has been added for weak typed languages like Python and JavaScript:
For JavaScript this resulted in the creation of TypeScript and for Python this is implemented in the [*typing* library](https://docs.python.org/3/library/typing.html).
Please also see [PEP 484](https://www.python.org/dev/peps/pep-0484/) for more details on type hints.

> To check if typing is done properly, you may use [`mypy`](http://mypy-lang.org/) library.
> **Remark:** Since the type hinting of Python was added later to the programming language, it's not a *first class citizen* feature.
> So don't expect the same safety net quality as from a strongly typed language.
> Also many external libraries do not implement proper typing in their APIs which makes it often cumbersome to use external libraries in combination with consequential typing.
> Still we recommend the usage of type hinting in Python code, since it helps avoid some of the most simple / stupid coding error.

### Test-Driven Development

[Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) (or short TDD) is a practice derived from Extreme Programming (XP).
The basic idea of TDD is to write (unit)tests first and then the actual code.
The key steps to add a new functionality are as follows:

1. Write (unit)tests about the new functionality.
2. Make sure the newly added tests are failing on the existing code.
(If not go back to step 1)
3. Adjust the existing code and implement the new functionality.
4. Ensure all tests (both old ones and your newly added ones) are passing.
(If not go back to editing your code.)
5. Repeat the previous steps for the next functionality.

The hardest challenge about TDD is a mental one:
You as a developer need to adjust your current programming flow and adopt to TDD.
In this sense it is comparable to [Java's Yoda conditions](https://en.wikipedia.org/wiki/Yoda_conditions).

We recommend giving TDD a try, because it makes development much more stable and reduces the fear of change, especially in larger software projects.
It also gives you the option to try new ideas when implementing, since you have the safeguard of your unit tests.
E.g. to some what safely try out concurrent execution, you need a good test basis, which TDD will give you.

### Pair Programming & Code Review

Developers are humans, so are data scientists, architects and ML engineers.
Like all humans, we all make mistakes.
To prevent mistakes making it into the final software product, software engineering has two additional safeguards (aside from testing).

#### Pair Programming

[Pair Programming](https://en.wikipedia.org/wiki/Pair_programming) basically describes two people working on one computer - one is coding and the other observes the produced source code.
As the coder codes tiny mistakes (e.g. using the wrong variable) may slip them, however for the observer this mistake is typically obvious.
By letting the coder know these tiny mistakes can be fixed immediately.
Aside from fixing simple mistakes the pair can validate each other's thinking, e.g. discuss if a certain coding pattern might make sense or what the best approach for a certain issue is.
There should be a lot of interaction between the two.

Pair Programming can also be used to educate junior resources by pairing them up with a senior resource.
The key here is to let the junior do the coding.

> **Note:**
> Pair programming does not prevent all possible mistakes.
> Two people can make the same mistakes.
> Therefore, it is important to have multiple safeguards.
> **Pair programming is not a valid replacement for testing!**
> **It is an addition.**

#### Code Review

Code reviews are also following the 4 eyes principle, but unlike pair programming code reviews happen delayed:

1. The developer writes tests and code (see TDD).
2. The developer writes a meaningful change log  / commit message and commits his/her changes.
3. The code reviewer reads through the change log / commit message.
(If they don't understand the change description, the changes are rejected and sent back to the developer to fix it.)
4. The code reviewer reviews the actual (code) changes and matches them to the change description.
(If the code changes are not good enough, the change is rejected and sent back to the developer to fix it. Some examples: wrong formatting, no tests, hard to understand code, functionality is not working, code changes do not match change description.)
5. If everything is fine, the change is approved by the code reviewer.

The above steps are very high level. For a more detailed view, please have a look at the [Google Engineering Practices Documentation](https://github.com/google/eng-practices), which contains a detailed language-generic description of how to do code reviews as a reviewer and also how to prepare a change to optimize for a smooth code review.

### Further Reading Material - "Clean Code" by Robert C. Martin

 > Even bad code can function. But if code isn't clean, it can bring a development organization to its knees. Every year, countless hours and significant resources are lost because of poorly written code. But it doesn't have to be that way.

This book is software engineering focussed and doesn't talk to much about AI. Nevertheless, we believe that data science code also needs to be clean; maybe even more so, since AI is usually much more complex then regular programming.

- [Clean Code at oreilly.com](https://learning.oreilly.com/library/view/clean-code/9780136083238/)
- [Cheat Sheet at plantgeek.ch](https://www.planetgeek.ch/wp-content/uploads/2013/06/Clean-Code-V2.2.pdf)

### Examples

To find examples for these guidelines and how to structure your code, go to the example repository: [MLOps pipeline](https://github.ibm.com/datascience-ibm/example-mlops-model-pipeline).

Specifically take a closer look at the oncology_model sources in `src/oncology_model`.
In the data model (`datamodel.py`) you find two classes, which represent a specific prediction and a tumor:

```python
class Prediction(Enum):
    BENIGN = "benign"
    MALIGNANT = "malignant"

    ...

class Tumor(NamedTuple):
    clump_thickness: int
    uniformity_of_cell_size: int
    uniformity_of_cell_shape: int
    marginal_adhesion: int
    single_epithelial_cell_size: int
    bare_nuclei: int
    bland_chromatin: int
    normal_nucleoli: int
    mitoses: int

    ...
```

The classes have methods and attributes, which closer define the object - more detail on that can be found in the examples section of [Chapter 14 - Cloud-Deployment (Deploy)](./cloud_deployment.md#chapter-14---cloud-deployment).

In this case we use this to clearly define, what a prediction looks like in our case:
It can only "benign" or "malignant". This gives the consumer of the prediction a clear interface to work with.

Also the tumor, which is the input for the prediction, is clearly defined in a class.
It consists of specific attributes, which are required for the model to make a prediction.
This way, the consumer can work with a clearly defined interface, which exactly states the input values for the model.

This is an example how to use code structuring in Python to implement interfaces between two parties.

In the `src/oncology_model` directory you will also find two additional files:

- `__init__.py`: Is the marker for Python to show, that this sub-folder is a package that can be imported using `import oncology_model`.
The file can be just empty or contain some initialization code for the package, when imported.
- `py.typed`: Is a marker for Python that shows, that this package has type information and can be checked for type errors.

```python
@classmethod
def from_class(class_, id: int) -> 'Prediction':
    switch = {2: Prediction.BENIGN, 4: Prediction.MALIGNANT}
    value = switch.get(id)
    if not isinstance(value, Prediction):
        raise ValueError(f"'id' seems to be an invalide class id, was:{id}")
    return value
```

The above class method from the `Prediction` class carries type information for the input type `id`, which should be int.
The return value of the method will be of type `Prediction`.
This class method generates a prediction object from the `Prediction` class.
