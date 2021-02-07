---
title: "Upgrading the Ada Language Server in Visual Studio Code"
description: "Instructions for upgrading the Ada Language Server in Visual Studio Code on Ubuntu"
published: true
date: 2021-02-07
tags : [ "ada", "language", "ada-lang", "format", "als", "ada_language_server", "source code" ]
categories : ["development", "ada", "source code" ]
---
# Upgrading the Ada Language Server for Visual Studio Code (VSC)

## Quick Solution Overview

The ***TL;DR*** version is:

1. Download the latest ALS pre-compiled binary version: https://dl.bintray.com/reznikmm/ada-language-server/linux-latest.tar.gz 
2. Replace the directory: `~/.vscode/extensions/adacore.ada-22.0.3/linux/` with the one included in the downloaded archive file. **NB:** version of '`adacore.ada-22.0.3`' sub directory name may vary!

For more of an explanation &mdash; grab a coffee (or other beverage) and keep reading!

## What is the 'Ada Language Server'?

The *Ada Language Server* (ALS) is described on its [GitHub page](https://github.com/AdaCore/ada_language_server) as:

> an implementation of the Microsoft Language Server Protocol for Ada/SPARK.

In simplistic terms the server can be used to check and help manage the source code of a project &mdash; written in the Ada language in this case! The ALS is used in conjunction with a source code editor or Integrated Development Environment (IDE). A brief list of functionality it can offer includes:

- auto complete;
- go to definition;
- find all references
- documentation on hover.

For more detailed information on the underlying *Language Server Protocol (LSP)* used by ALS itself, can be found here: [Microsoft Language Server Protocol](https://microsoft.github.io/language-server-protocol/).

Another benefit of the LSP is that it provides a common interface and communications protocol between a code editor program and the specifications and application of language rules and support. So LSP servers, such as the *Ada Language Server* (ALS), can be used by many different editors to provide a common set of support and functionality via one program.

For example the *Ada Language Server* supports commonly used editors such as: [Neovim](https://neovim.io/); [Visual Studio Code](https://code.visualstudio.com/); [GPS](https://github.com/AdaCore/gps); [emacs](https://www.gnu.org/software/emacs/); and [QtCreator](https://www.qt.io/product/development-tools). See the [ALS GitHub page](https://github.com/AdaCore/ada_language_server) for more information on support for these programs.


## Why Write this 'Upgrade' Article?

The development of the *Ada Language Server* is open source and is actively maintained by [AdaCore](https://www.adacore.com/) on the GitHub page here:

- [AdaCore Ada Language Server](https://github.com/AdaCore/ada_language_server)

If you check the [issues page](https://github.com/AdaCore/ada_language_server/issues) there are many helpful posts (open and closed) on fixing problems you may encounter when using Visual Studio Code with the ALS extension. 

For example, when I first started using it, the ALS would frequently pop up with error, and complaints about the code, sometimes crash and so on, which was frustrating to say the least. Consequently, when I was reading through the various issues, I discovered the latest version of ALS can be installed easily.

For me, that upgrade created stability and usability benefits immediately! 

[AdaCore](https://www.adacore.com/) and the community have done a great job to provide this functionality, that makes the Ada development experience modern and pleasant to code. Whats more, the required files to carry out an upgrade are pre-built and provided for easy download and upgrade. You can of course also download the ALS source and build it yourself too if you prefer!

While the links to download the latest version of ALS were clear in many of the issues I read, and on the main 'README' page, there is link to a `.vslx` file too, it took me a bit of figuring out on 'how' and 'where; to install the latest version. This article is to help others who may experience the same problems.

## Upgrading on Ubuntu

Below will describe the assumptions, provide some background and context, and the steps to perform to upgrade for ALS on a computer running VSC and Ubuntu desktop.


### Upgrading Assumptions

This article will explain how to upgrade the *Ada Language Server* when used on a computer specifically with:

- [Ubuntu 20.04.2 LTS](https://ubuntu.com/download/desktop) - Linux desktop installation running on AMD x64 processor;
- [Visual Studio Code (VSC)](https://code.visualstudio.com/) - being used as the source code editor to develop Ada programs;
- [Language Support for Ada](https://marketplace.visualstudio.com/items?itemName=AdaCore.ada) - a *VSC Marketplace* extension to provide both Ada and SPARK language support in the Visual Studio Code program.
- [GNAT Ada Compiler and tools](http://www.getadanow.com) - an installed and working Ada compiler and tools. Also see a previous post also: [Installing the FSF GNAT Ada Compiler on Ubuntu](https://www.wiremoons.com/development/ada/install/2020/12/19/Install-GNAT-Ada-Compiler-on-Ubuntu.html).


### ALS File Location

It is assumed that you have a working Ubuntu Linux computer with the above (or compatible similar software) installed and working as well. The following instructions focus on upgrading the *Ada Language Server* that is provided with the installation of the [Language Support for Ada extension](https://marketplace.visualstudio.com/items?itemName=AdaCore.ada).

As an aside, the Ada *VSC Marketplace* extension can be installed from a command line prompt with the command (assuming VSC is installed in the default location of: `/usr/share/code/bin/code`):

```bash
/usr/share/code/bin/code --install-extension adacore.ada
```

Once the [Language Support for Ada extension](https://marketplace.visualstudio.com/items?itemName=AdaCore.ada) is installed, its default installation location is a sub directory alongside all the other extension that may be in use: `~/.vscode/extensions/`


The actual sub folder the *Language Support for Ada extension* is located in will change dependant on the version. The current version (as of 07 Feb 2021) is named: '`adacore.ada-22.0.3`'. The full path is therefore:

```
~/.vscode/extensions/adacore.ada-22.0.3/
```

The actual language server program itself is in a further sub directory called: '`linux`'. 

### Upgrade Steps

A summary of the actions to be taken to carry out the upgrade are below:

1. Download the latest pre-built version of the ALS from the GitHub page;
2. Make sure Visual Studio Code (VSC) is not running;
3. Back up the current VSC extension installed version of the ALS;
4. Install the new version obtained from GitHub in step one above.


### Upgrade Detailed Commands

First exit VSC - just so it is not using any of the files that are to be upgraded. Then open a command prompt (ie *Terminal* in Ubuntu Gnome desktop).

The commands shown below are those that need to entered into the command line prompt, to carry out the upgrade.

All lines shown in the example command below, starting with '`#`' are just comments for information only.

**ONE** : Download the latest version of the ALS binary archive file for Linux AMD x64. The links to the pre-compiled binaries are included in the '*Install*' section of the [ALS GitHub README](https://github.com/AdaCore/ada_language_server#install). There are binaries for *macOS*; *Windows x64*; and *Linux x64*. The later will be used here. The file will be download to the current directory by the `curl` command shown &mdash; so a new temporary working directory is created first called: `~/scratch`

```bash
# make sure we are in our $HOME directory:
cd ~

# create a temporary working directory called: 'scratch'
mkdir ~/scratch

# change into the new 'scratch' directory:
cd ~/scratch

# now download the latest binary verion of ALS for Linux
curl -OL https://dl.bintray.com/reznikmm/ada-language-server/linux-latest.tar.gz 
```

**TWO** : Backup the existing ALS VSC extension installation.

```bash
# change into the VSC extension directory
cd ~/.vscode/extensions/

# check the directory name of 'AdaCore.Ada' extension version:
ls adacore*

# The version found was:  'adacore.ada-22.0.3' - alter the command 
# below to match the name of the one on your computer:
cd adacore.ada-22.0.3/

# Back up the Linux ALS version currently installed that is located
# in the sub directory called 'linux'. Back up to a 
# new directory called 'linux-original'
mv ./linux ./linux-original
```

**THREE** : Install the latest version of the ALS program as downloaded in step one above. The downloaded version includes more files than the original one (ie as backed up in step 2 above). This is to insure the Ada packages (libraries) it was built with are also available - just in case they differ from those versions on your computer. The contents of the archive file need to be expanded (decompressed) and must be in a sub directory named '`linux`' &mdash; basically to replace the original installed version we backed up above. The archived files are contained in a '`linux`' sub directory already - so we only need to decompress it in the right location.

```bash
# check we are still in the right directory, where output
# will be similar to: 
#  /home/<username_here>/.vscode/extensions/adacore.ada-22.0.3/
pwd

# copy the downloaded archive file (from '~/scratch' directory) 
# to the the extension directory we are located in - picking 
# up from the last command entered in step 2 above
cp ~/scratch/linux-latest.tar.gz .

# decompress the archive file into a new directory called: 'linux'.
# The downloaded archive includes the directory already - so no need
# to create it first!
tar -xzvf ./linux-latest.tar.gz
```

*FOUR* : Check the contents of the `linux` sub directory to ensure the ALS 
files we expect are there.

```bash
#  Continuing on from step three above - run the command
ls ./linux

# below is the content seen as output on my computer in the folder:
#
#  ~/.vscode/extensions/adacore.ada-22.0.3/linux/
#
ada_language_server*  libgnarl-2020.so*  libgnatcoll_gmp.so.0*
libgnatcoll.so.0*  liblangkit_support.so*  libxmlada_input_sources.so.2020*  libxmlada_schema.so.2020* libadalang.so*  libgnat-2020.so*  
libgnatcoll_iconv.so.0* libgpr.so*  libxmlada_dom.so.2020*
libxmlada_sax.so.2020*  libxmlada_unicode.so.2020*
```

That's it! Once those files are in place in the `linux` sub directory, then you can exit the command prompt, and start using VSC. It should pick up and use the new version of the ALS downloaded from the GitHub site.

## Another Common VSC and Ada Project ALS Related Issue

Some of the errors you may see if you are editing an Ada project with VSC can be '*fixed*' by ensuring the project file is specified in the local projects VSC '`settings.json`' file.

There may already be in the root folder of you Ada project a hidden sub directory called: '`.vscode`'. An example project structure might look as below:

```
aproject/
├── alire.toml
├── aproj.gpr
├── bin
├── .git
├── .github
│   └── workflows
│       └── aproj-build-ada.yml
├── .gitignore
├── LICENSE
├── obj
├── README.md
├── src
│   ├── aproj.adb
│   ├── source1.adb
│   └── source1.ads
└── .vscode
    └── settings.json
```

If the '`.vscode/` folder is missing it can be created with the command:

```bash
mkdir .vscode
```

The contents for the '`settings.json`' located in the folder may vary &mdash; but try adding the following line, but change the '`aproj.gpr`' to match your own projects file name:

```json
{
    "ada.projectFile": "aproj.gpr"
}
```
If you want more verbose output from the ALS (ie perhaps to trouble shoot an issue) then the following additional line can be added too:

```json
{
    "ada.trace.server": "verbose",
    "ada.projectFile": "apass.gpr"
}
```

If you are having problems with ALS, visit and search the '*open*' and '*closed*' [issues](https://github.com/AdaCore/ada_language_server/issues) first, as you may find someone else has had a similar (or the same) problem already.


## How to Get More Ada Information

More general information and support for installing Ada can be found at the site:

- [Get Ada Now](http://www.getadanow.com)

The *AdaCore* tools and libraries are all on GitHub here:

- [AdaCore GitHub Site](https://github.com/AdaCore)
    
There are plenty of good articles for more background on Ada such as:

- [Wikipedia Ada Language page](https://en.wikipedia.org/wiki/Ada_(programming_language))    
- [AdaCore - resources page](https://www.adacore.com/resources)

One great free resource to learn the Ada language, so you can write your own programs is available here:

- [Learn Ada Course](https://learn.adacore.com/courses/intro-to-ada/index.html)

You can also download a nice PDF book to read of the above course here:

- [Learn Ada Course (PDF)](https://learn.adacore.com/pdf_books/courses/intro-to-ada.pdf)


### Article Details

- Title: *{{ page.title }}*
- Published Date : {{ page.date | date: "%d %b %Y" }}
