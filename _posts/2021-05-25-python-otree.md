---
layout: post
title: "Installing Python and oTree"
subtitle: ""
date: 2021-05-25
header-style: text
tags:
    - Python
    - oTree
---

This is a simple log of how I install Python and [oTree](https://www.otree.org/).

I use [Anaconda](https://www.anaconda.com/), "a distribution of the Python and R for scientific computing, that aims to simplify package management and deployment".
First, create a new virtual environment named "otree" (or whatever), in which we will install oTree later, to separate from the default "base" environment:
```zsh
$ conda create --name otree
```
This new environment has no package installed.

Activate “otree” environment:
```zsh
$ conda activate otree
```

Install pip inside "otree" to install oTree later, as oTree is not available from conda.
```zsh
$ conda install pip
```
Then, pip and some of its dependencies will be installed.

Alternatively, we can create the environment with pip installed using a single command:
```zsh
$ conda create -n otree pip
```
To exit the current environment and get back to the base environment, we could execute
```zsh
$ conda deactivate
```

Then we install oTree in the "otree" environment.
First enter the "otree" environment:
```zsh
$ conda activate otree
```
Then use pip to install oTree:
```zsh
$ pip3 install -U otree
```

### Run oTree
It seems that the following command
```zsh
$ /Applications/Python\ 3.9/Install\ Certificates.command
```
is undoable (or unnecessary?) if we are running oTree in a virtual environment rather than the default environment of our system.
(If you downloaded Python from its official website, this command should probably be a step of the installation.)

Then go to wherever you'd like to put your oTree project
```zsh
$ cd some_path
```
and create your project:
```zsh
$ otree startproject myproject
```
It asks you
```zsh
Include sample games? (y or n):
```
Type either `y` or `n` and then it shows
```shell
Created project folder.
Enter "cd myproject" to move inside the project folder, then start the server with "otree devserver".
```
Therefore,
```zsh
$ cd myproject
$ otree devserver
```
Then we can visit the demo site at http://localhost:8000/.
To stop the server, press <kbd>⌃ Control</kbd> + <kbd>C</kbd> at the command line.


### A tip
After installing Anaconda, when we restart the Terminal, there would be a "(base)" appearing in front of Terminal prompt looking like:
```zsh
(base) $
```
This is because conda's base environment is activated on startup.
To disable it, type
```zsh
$ conda config --set auto_activate_base false
```
and restart Terminal.
Then every time we open Terminal, the conda environment will not be activated automatically and we need to run `conda activate` to use all the packages inside Anaconda like the latest `python` (other than the system default one), `jupyter-notebook`, and so on.

(Reference: [How to remove (base) from terminal prompt after updating conda](https://stackoverflow.com/questions/55171696/how-to-remove-base-from-terminal-prompt-after-updating-conda).)