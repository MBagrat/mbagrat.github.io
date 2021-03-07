---
layout: post
title: "Haskell, Stack and Intellij IDEA IDE setup tutorial"
author: bagrat
categories: [ Programming Languages ]
image: assets/images/haskell-stack-and-intellij-idea/haskell-stack-and-intellij-idea.png
tags: [haskell, stack, intellij-idea, functional-programming]
---

Upon completion you will have a sane, productive Haskell environment adhering to best practices.

## Basics

* Haskell is a programming language.
* Stack is tool for Haskell projects. (similar tools for other languages include Maven, Gradle, npm, RubyGems etc)
* Intellij IDEA IDE is a popular IDE.

## Install required libraries  (if you are using linux)

```bash
  sudo apt-get install libtinfo-dev libghc-zlib-dev libghc-zlib-bindings-dev
```

## Download and extract Stack

Don't install Haskell, Stack, Cabal or any other Haskell tool or library using your OS package manager or using Cabal.

Instead, download Stack from this [link for your os](https://docs.haskellstack.org/en/stable/install_and_upgrade){:target="_blank"} and extract it
into `~/.local/bin` (ie the complete path to stack should be `~/.local/bin/stack`) and add `~/.local/bin` to `$PATH`. (
adding a folder to `$PATH` is beyond the scope of this tutorial, search and you will find)

## Use Stack to create, build and run a project

Go to the folder where you want to create your Haskell project and run

```bash
  stack new my-project
  cd my-project
  stack setup
  stack build
  stack exec my-project-exe
```

Stack installs Haskell (ghc) to `~/.stack/programs/x86_64-linux/ghc-7.10.3/bin/ghc`, project-specific dependencies to
appropriate locations inside the project, and builds and runs the project.

## IDE

* Make sure you're not inside a Haskell project folder and run

```bash
  stack install hindent stylish-haskell
```

to install tools required by the IDE plugin for Haskell. (they will be installed to `~/.local/bin`)

* Download and Intellij IDEA IDE from [jetbrains.com](http://www.jetbrains.com/idea){:target="_blank"} and extract it to some appropriate folder.
* (**NOTE** there are a number of different mutually exclusive Haskell plugins. If other Haskell plugins are already
  installed, uninstall them before installing the `Intellij-Haskell` plugin) Start IntelliJ up and install the
  plugin `Intellij-Haskell`. (The plugin will install its own copies of Intero and Hlint)
* Restart IntelliJ.
* `Settings` | `Other` | `Haskell` | add the respective paths to the tools (eg for `hindent` `~/.local/bin/hindent`)
* File | New | Project from Existing Sources | in the New Project wizard select Import project from external module and
  check Haskell Stack
* In next page of wizard configure Project SDK by configuring Haskell Tool Stack with selecting path to stack binary,
  e.g. `/usr/local/bin/stack`
* Finish wizard and project will be opened.

Wizard will try to automatically configure which folders are sources, test and which to exclude. Plugin will
automatically build Intero and HLint to prevent incompatibility issues (If you use non LTS or Nightly resolver e.g.
ghc-7.10.2, you may have to build them manually since there are some extra-deps should be added to stack.yaml). Those
tools are built against Stackage release defined in project's stack.yaml. If you want to use later version of tool, you
will have to build tool manually in project's folder by using stack build.

* Check Project structure/Project settings/Modules which folders to exclude (like .stack-work and dist) and which
  folders are Source and Test. (normally src and test)

Plugin will automatically download library sources (since Stack version 1.2.1 also for test dependencies). They will be
added as source libraries to module. This option gives you nice navigation features through libraries. Sources are
downloaded to folder .ideaHaskellLib inside root of project. After changes to dependencies you can download them again
by using `Tools` | `Download Haskell Library Sources`. **This is what enables viewing docs while coding.** It creates a
folder `ideaHaskellLib` in which it runs `stack unpack` for every dependency output by `stack list-dependencies`. **
Remember to repeat this step on any change in dependencies.**

* Add the `ideaHaskellLib` folder to the `.gitignore` file for the project (it's not part of your projects source code).

In the background for each Haskell project two Stack repls are running. You can restart them by Tools/Restart Haskell
Stack REPLs. When you make large changes to stack.yaml or Cabal file, you have to restart IntelliJ project.

Most or all of the usual features of Intellij like auto-completion, go to declaration `ctrl+b` etc should now work.

## MISC

### Development flow

#### Build and run the project

Terminal has to be used for this. Go to `path to root of my-project` and run

```bash
  stack build && stack exec my-project-exe
```

#### Interactive interpreter

One of the advantages of Haskell is the interactive interpreter. Go to `path to root of my-project` and run

```bash
  stack ghci
```

to fire up the interpreter. The Main module and other project modules should be automatically loaded. With the modules
loaded, inside the interpreter, run

```bash
  main
```

to interactively call the main function.

New functions can be defined and tried out interactively inside the interpreter. When editing source files, run

```bash
  :r
```

inside the interpreter to reload them. (**NOTE** reloading edited source files into the interpreter does only that-
rebuilding the project is still necessary to reflect source file edits in the built project)

### Updating Haskell and Stack

Stack can install and manage multiple Haskell versions, and different versions of different libraries in different
projects. Stack itself can be upgraded to the latest version simply by running

```bash
  stack upgrade
```

This is why neither Haskell, Stack, Cabal nor any other Haskell tool or library should be installed and managed through
the OS package manager or Cabal. (anything installed and managed through the OS package manager will be a single, global
version, and the versions are often lagging severely)

### Other IntelliJ plugins for Haskell

As previously seen, there are other IntelliJ plugins for Haskell that may or may not be better than, have more features
etc than the `Intellij-Haskell` plugin, but I've been unable to get any of them running. Which brings us to...

### What went in to this Gist

I tried several times to pick up Haskell, but always gave up, not due to anything to do with the language itself, but
due to not being able to set up a sane, productive environment. After lots of time, energy and frustration it appears as
if I've finally figured out how to do it. This Gist is for my own future reference, but I hope others will find it
useful as well, and that others won't have to go through what I went through just in order to get started.