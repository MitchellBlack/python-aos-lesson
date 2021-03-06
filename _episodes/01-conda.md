---
title: "Software installation using conda"
teaching: 20
exercises: 5
questions:
- "How do I install and manage all the Python libraries that I want to use?"
objectives:
- "Explain the advantages of Anaconda over other Python distributions."
- "Extend the number of packages available via conda using conda-forge."
- "Create a conda environment with the libraries needed for this workshop."
- "Upload that environment to Anaconda Cloud so that others can install it." 
keypoints:
- "Use conda to install and manage your Python environments."
---

## Background

The Python package installer ([pip](https://pip.pypa.io)) only works for libraries written in pure Python.
Many scientific Python libraries have C and/or Fortran dependencies,
so the easy solution to this problem is to use a distribution like Anaconda or Canopy, 
which come with all the most popular libraries pre-installed.
These distributions also come with a package manager for installing libraries that weren't pre-installed.
This tutorial focuses on [conda](https://conda.io/docs/),
which is the package manager associated with Anaconda
(as we'll see, it's better than the Canopy package manager).

## Basic usage

Anaconda comes with around 75 of the most widely used libraries (and their depedencies).

In addition, there are around 330 libraries available via `conda install`,
which can be installed via the Anaconda Navigator graphical user interface or at the command line.
For instance, installing the popular `xarray` library can be achieved
by simply entering the following at the command line:  
~~~
$ conda install xarray
~~~
{: .language-bash}

You can use `conda search -f xarray` (or the Navigator)
to find out if the packge you want is in the 330.

> ## Miniconda
>
> If you don't want to install the entire Anaconda distribution,
> you can install [Miniconda](http://conda.pydata.org/miniconda.html) instead.
> It essentially comes with conda and nothing else.
>
{: .callout}


## Advanced usage

This is all great, but up until now Anaconda gives us nothing that Canopy doesn't.
The real advantage of Anaconda is the [Anaconda Cloud](https://anaconda.org) website,
where the community can contribute conda installation packages.

You can search Anaconda Cloud
to find the command line entry needed to install the package. e.g:
~~~
$ conda install -c https://conda.anaconda.org/scitools iris
~~~
{: .language-bash}

In many cases, there are many versions of the same package up on Anaconda Cloud.
To address this problem, [conda-forge](https://conda-forge.github.io/)
has been launched to have a central place for just a single version.
You can therefore expand the selection of packages available via `conda install`
beyond the chosen 330 by adding conda-forge:
~~~
$ conda config --add channels conda-forge
~~~
{: .language-bash}

### Environments

Now we can go ahead and use conda to install the libraries we need for this lesson.
Rather than install everything in the same place
(which can get unweidly if you've got mutliple data science projects on the go)
it's common practice to create separate environments
for the various projects you're working on. 

Let's call this environment `pyaos-lesson`
and include the [jupyter](https://jupyter.org/) library (so we can use the jupyter notebook),
[iris](http://scitools.org.uk/iris/) (for handling our CMIP5 data),
[cmocean](http://matplotlib.org/cmocean/) (for nice color palettes) and 
[gitpython](http://gitpython.readthedocs.io)
(for integrating version control information into data provenance):

~~~
$ conda create -n pyaos-lesson jupyter iris cmocean gitpython
$ source activate pyaos-lesson
~~~
{: .language-bash}

> ## Create your own environment
>
> Go ahead and create your own `pyaos-lesson` environment to use in this workshop:
> ~~~
> $ conda config --add channels conda-forge
> $ conda create -n pyaos-lesson jupyter iris cmocean gitpython
> $ source activate pyaos-lesson
> ~~~
> {: .language-bash}
{: .challenge}

If we list all the libraries in this new envrionment,
we can see that jupyter, iris, cmocean, gitpython
and all their required dependencies have been installed:

~~~
$ conda list
~~~
{: .language-bash}

(it's `source deactivate` to exit)

You can have lots of different environments:

~~~
$ conda info --envs
~~~
{: .language-bash}

and you can export them (to a YAML configuration file) for others to use:

~~~
$ conda env export -n pyaos-lesson -f pyaos-lesson
~~~
{: .language-bash}

You can then upload the environment to your account at Anaconda Cloud:

~~~
$ conda env upload -f pyaos-lesson
~~~
{: .language-bash}

so that others can re-create your environment as follows:

~~~
$ conda env create damienirving/pyaos-lesson
$ source activate pyaos-lesson
~~~

> ## Deleting things
>
> To delete an environment:
> ~~~
> $ conda env remove -n pyaos-lesson
> ~~~
> {: .language-bash}
> To delete Anaconda, just find the Anaconda folder on your system and delete it.
{: .callout}

> ## conda kapsel
>
> To make the management and sharing of environments even easier,
> [conda kapsel](https://www.continuum.io/blog/developer-blog/automate-your-readme-conda-kapsel-beta-1)
> has been released.
>
{: .callout}
