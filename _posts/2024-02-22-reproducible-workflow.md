---
title: "Using a cookiecutter and make for a reproducible workflow"
layout: single
excerpt: "I promise that title will make sense"
tags: [til, data-science]
---

Everyone would love their workflow to be entirely reproducible, where a newcomer could generate the data, figures and report with as little pain as possible. The problem is, data science projects are often messy. You normally start off with a data source, make a few exploratory investigations, and by the time you get to writing the report, there are hundreds of these notebooks and figures dotted around the file system.

Now, I could be disciplined and make sure the workspace is kept tidy, but frankly that requires a lot of effort (that I simply don't have the patience for). Besides, the flurry of data exploration and creative problem solving is all part of the process that we should encourage.

As you have probably guessed, some smart *cookie* has already *made* tools to solve this problem... read on to understand just how terrible those puns are.

# Making a cookiecutter

We will be using two tools to create our reproducible workflow: [cookiecutter](https://www.cookiecutter.io/) to stamp out our folder structure and then [make](https://www.gnu.org/software/make/) our data processing and analysis automated. You can use my cookiecutter with the following (which is based on this [original](http://drivendata.github.io/cookiecutter-data-science/)):

```shell
cookiecutter https://github.com/rlaker/cookiecutter-data-science
```

The first tool, [cookiecutter](https://www.cookiecutter.io/), allows us to define the folder structure and include any boilerplate code that can streamline the process of creating the project. For example, with [cookiecutter](https://www.cookiecutter.io/) we can install and set up:
- a [conda](https://docs.conda.io/projects/conda/en/stable/user-guide/getting-started.html) environment for this project with an `environment.yaml` file. Simply run `make environment` to install some common packages, e.g. pandas or matplotlib
- [pre-commit](https://pre-commit.com/) hooks to be installed automatically - ensuring our formatting is consistent
- a python package for each project (which lives in the `src` folder). This means you can install the custom package into any notebook in the project without having to worry about paths to a local folder (see these blog posts [here](https://www.ethanrosenthal.com/2022/02/01/everything-gets-a-package/) and [here](https://ericmjl.github.io/blog/2022/3/31/everything-gets-a-package-yes-everything-gets-a-package/)). This also makes the project more portable when it comes to productionising the model.
- include custom style files, such as the one from my previous [post](https://www.ronanlaker.com/matplotlib-stylesheet/) 
- a place to store the raw `data`, `notebooks`, `models`, `figures` etc.

As well as helping us remember where all the necessary files are stored, such a consistent folder structure can also allow us to write all the necessary boilerplate in a `makefile`. Essentially, this file holds the terminal commands needed for [make](https://www.gnu.org/software/make/) to reproduce the outputs of the project. Not only are makefiles **both human and machine readable documentation**, but they can specify the dependencies for each part of the project, i.e. the Latex report is dependent on the code to make the figures, which depends on the data processing script. By looking at the file timestamps, [make](https://www.gnu.org/software/make/) can also infer which files need to be updated, only running the necessary parts of the pipeline.


![](/images/workflow/workflow_diagram.svg)

While storing pre-processed versions of data files can save disk space, I learnt the hard way why this is a bad idea. **Do not** manually edit or change the raw data, you will forget what you did in a few weeks/months. Instead, create a pipeline and document the steps in the `makefile`. Even if you forget any of the steps to your implementation, you only need to run `make data`. 

> If you follow convention, then a completely new user could recreate any of your projects by just typing `make all`

The power of make, combined with a consistent folder structure, can also enable you to automatically create documentation for your project with a single command. For example, running `make docs_html` in the newly created project will use [pdoc](https://pdoc3.github.io/pdoc/) to automatically generate a static website from the docstrings within the code!

# Further Reading

If you want a more in depth review of this type of workflow:
- [Original cookiecutter](http://drivendata.github.io/cookiecutter-data-science/)
- [Make tutorial](https://gertjanvandenburg.com/files/talk/make.html)
- [Why use make](https://bost.ocks.org/mike/make/)
- [Example make workflow](http://zmjones.com/make/)

