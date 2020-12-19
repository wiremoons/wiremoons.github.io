---
title: "Installing the FSF GNAT Ada Compiler on Ubuntu"
description: "Installing the Ada language GNAT compiler on Ubuntu"
published: true
date: 2020-12-19
tags : [ "ada", "language", "ada-lang", "ubuntu", "install", "gnat" ]
categories : ["development", "ada", "install" ]
---
# Installing the FSF GNAT Ada Compiler on Ubuntu

## GNAT Complier Background

The *GNU NYU Ada Translator (GNAT)* is an open source *Ada* language compiler that works on Linux, Windows and macOS, as well as other operating systems too.

This *Ada* language compiler is part of the more widely known *GCC* compiler suite, that is better recognised for its *C* and *C++* language support. GNAT supports all versions of the Ada language, i.e. Ada 2012, Ada 2005, Ada 95 and Ada 83. 

The GNAT project started in 1992 when the *United States Air Force (USAF)* awarded *New York University (NYU)* a contract to build a free compiler for *Ada* to help with the Ada 9x standardization process. The three million dollar contract required the use of the *GNU GPL* for all development, and also it assigned the copyright to the *Free Software Foundation (FSF)*. The first official validation of GNAT occurred in 1995.

Today a company called [AdaCore](https://www.adacore.com/) provides [commercial products](https://www.adacore.com/products) based on GNAT, but they also continue to support the development of the open source GNAT compiler. *AdaCore* also make available a '*comunity*' version of their toolsets, libraries, and *GNAT Studio IDE* for anyone to use without a fee. 

The source code for the FSF GNU Ada compiler and excellent supporting tools are available from:

- [GNU GNAT Compiler Site](https://gcc.gnu.org/wiki/GNAT)
- [AdaCore GitHub Site](https://github.com/AdaCore)


## GNAT vs AdaCore Communuty Edition

It is nice to have choices &mdash; and the ability to use either the *AdaCore Community Edition* or the *FSF GNU GNAT compiler* directly is one such choice! 

How you decide will normally be based on:

- *AdaCore* GPL license restrictions;
- The computer platform to be used for development;
- The computer platform to run the program on;
- Ease of installation, setup, and support.

If you use an open source operating system such as *Linux*, then the *FSF GNAT compiler* can normally also be installed via the native package manager. This ensures *Ada* language support is provided to all computer platforms supported by Linux &mdash; which if you are not using a mainstream Windows, or macOS operating system is really important.

*AdaCore* only provides their '*Community*' edition to run on *Apple macOS* using Intel AMD64 CPUs, *Linux* and *Windows* when using x86/AMD64 CPUs. So, if for example you have a *Raspberry Pi 4B*, *Surface Pro X*, or *MacBook Air M1* you would be without an *AdaCore* development desktop environment. You may however by able to cross compile programs to run on these computer from a supported one. This of course may change in the future, but is a current limitation of the *AdaCore Community Edition* &mdash; which is understandable as *AdaCore* primarily support platforms needed by their commercial customers.

This article will show the steps to install *FSF GNAT* support on *Ubuntu 20.04 LTS* using the native package manager. The steps have been used to be able to build *Ada* language programs on both *Raspberry Pi 4B* computers and number of AMD64 CPU based laptops.

While I do like and use as my first preference the *AdaCore Community Edition* on both *Linux* desktop and *Windows*, once I have finsihed developing in this environment, I just pull the same source code onto my *Raspberry Pi 4B* computers, and then simply re-compile localy usng the Ubuntu package manager *FSF GNAT* compiler and tools. One of the major benefits for developing using open source tools and languages!


## Ubuntu 20.10 GNAT Installation

Installing basic *FSF GNAT Ada* language compiler support on Ubuntu *20.04* (or other releases too!) is done with the local package manager.

The command shown below can be entered into a *Terminal* window on *Ubuntu*. As with any use of the `sudo` command, your adminsirator or root password will need to be enter when prompted:

1. First ensure the latest packages are known to your computer: `sudo apt update`
2. Next install the GNAT compiler tools: `sudo apt install gnat`
3. Add *gprbuild* tool support and a debugger : `sudo apt install gprbuild gdb`
4. A handy extra is a copy of the current *Ada Lanaguge Reference Manual (ARM)*: `sudo apt install ada-reference-manual-2012`

That is a enough for a basic but robust Ada development environment that can be used from the command line to build basic programs.

However, it is also useful to have access to addtional supporting lanaguage libraries. These too can be added via the *Ubuntu* package manager as follows. The libraries suggested here are a personal choice, but do match up with most of those included by default with the *AdaCore Community Edition* installation too. The packages (ie libraries) are:

- *asis-programs* : a number of useful tools such as `gnatpp`, `gnatcheck`, `gnattest`, etc;
- *libaws19-dev* : Ada Web Server (AWS) and other useful related development files;
- *libcurl4-openssl-dev* : development library for SSL used by Ada Web Server (AWS);
- *liblzma-dev* : compression support development library;
- *libgnatcoll-gmp19* : part of the GNAT collection (GNATColl) general purpose Ada library;
- *libgnatcoll-gmp18-dev* : developement support for the GNATColl;
- *libgnatcoll18* : more support for the GNATColl;
- *libgnatcoll18-dev* : more development support for the GNATColl;
- *libgnatcoll-doc* : GNATColl documentation;
- *libgnatcoll-iconv18-dev* : GNATCall static library and Ada specifications
 for the binding with the Iconv C library;
- *libgnatcoll-readline18-dev* : GNATColl development support for Readline library;
- *libgnatcoll-sqlite18-dev* : GNATColl database support for SQLite development;

To install all the above, run the command:

```bash
sudo apt -y install asis-programs libaws19-dev libcurl4-openssl-dev liblzma-dev \
        libgnatcoll-gmp19 libgnatcoll-gmp18-dev libgnatcoll18 libgnatcoll18-dev \
        ada-reference-manual-2012 libgnatcoll-doc \
        libgnatcoll-iconv18-dev libgnatcoll-readline18-dev libgnatcoll-sqlite18-dev
``` 

To install everything explained above in a single command, you can run it as:

```bash
sudo apt -y install gnat gprbuild gdb asis-programs \
        libaws19-dev libcurl4-openssl-dev liblzma-dev \
        libgnatcoll-gmp19 libgnatcoll-gmp18-dev libgnatcoll18 libgnatcoll18-dev \
        ada-reference-manual-2012 libgnatcoll-doc \
        libgnatcoll-iconv18-dev libgnatcoll-readline18-dev libgnatcoll-sqlite18-dev
```

Your can make sure the installed tools and programs are all working, and see what versions are included by running the following command from the terminal:

```bash
gnatmake --version
gprbuild --version
gnatls -v
gnat --version
```

If you are running Ubuntu as a desktop, you can also add the *GNAT Programming Studio (GPS)* which is an Ada (and C) Intergrated Development Environment (IDE) as well. This provides a graphical development environment, built-in file editing; HTML based help system; complete compile/build/run cycle; intelligent source navigation; project management; and much more. This is also provided by default with the *AdaCore Community Edition*. To install it, run the following command in a Terminal:

```
apt install gnat-gps
```
 
Once installed, the *GNAT Programming Studio (GPS)* aplication will be available with all the other Ubuntu applications you have installed.

If you want to see more detialed information about any of the above Ubuntu packages before you install them on you computer, use the command: `apt info <packagename>`. So for example, to see information all about GPS, use the command:
```
apt info gnat-gps
```

That's it!!  Time to build or development some Ada programs...


## How to Get More Information

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
