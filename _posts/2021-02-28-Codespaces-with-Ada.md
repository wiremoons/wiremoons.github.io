---
title: "Using GitHub Codespaces with the Ada language"
description: "*Codespaces* test for a very basic *Ada* language project." 
published: true
date: 2021-02-28
tags : [ "ada", "language", "ada-lang", "codespaces", "visual_studio_code", "source_code" ]
categories : ["development", "ada", "source_code" ]
---
# GitHub Codespaces Test for the Ada Language

*Codespaces* is a GitHub facility that is currently being tested that aims to provided a development environment designed to be used from within a web browser. This outlines test for a very basic *Ada* language project, from within Codespaces.  

## Environment

I create a new test repository for the purposes of testing the new [GitHub Codespaces](https://github.com/features/codespaces) feature, that provides a *Visual Studio Code* development environment accessible from any computer with a web browser. 

The repository I created is [here](https://github.com/wiremoons/codespace-test-ada) - although there is not much to see really!

The container it uses to encapsulate the development environment is the [default Linux (Ubuntu)](https://github.com/microsoft/vscode-dev-containers/tree/v0.159.0/containers/codespaces-linux) one that is provided once the Codespace is launched.

This default environment includes support for quite a number of programming languages that includes: *Python*, *Node.js*, *JavaScript*, *TypeScript*, C*++*, *Java*, C*#*, F*#*, .NET *Core*, *PHP*, *PowerShell*, *Go*, *Ruby*, *Rust*. Unfortunately this list does not include support for *Ada*.

In order to provide Ada support so code and be check, tested, and built, an *Ada* development language environment is needed as well. These can be installed into the Codespace in two ways that as relatively easy that include:

1. Open the Codespaces (Visual Studio Code) built-in Terminal window and install the required support via the Ubuntu package manager called `apt`. To install the *GNAT FSF Ada Compiler*, build tools, and some commonly used libraries the following command can be executed. The command did not execute cleanly always, but also entering `sudo apt-get update --fix-missing` did fix the issues when it was re-run:

```shell
sudo apt install -y gnat gprbuild gdb upx-ucl asis-programs \
     libaws19-dev libcurl4-openssl-dev liblzma-dev \
     libgnatcoll-gmp19 libgnatcoll-gmp18-dev libgnatcoll18 libgnatcoll18-dev \
     libgnatcoll-doc libgnatcoll-iconv18-dev libgnatcoll-readline18-dev \
     libgnatcoll-sqlite18-dev
```

2. The other approach than should ensure the tools are installed and available when the Codespaces environment starts up is to added the required packages into the *Dockerfile*. The existing *Dockerfile* that is provided with the project can be replaced with the following to install the required *Ada* language support:

```dockerfile
FROM mcr.microsoft.com/vscode/devcontainers/universal:1-focal

# ** [Optional] Uncomment this section to install additional packages. **
USER root
#
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
     && apt-get -y install --no-install-recommends gnat gprbuild gdb upx-ucl asis-programs \
     libaws19-dev libcurl4-openssl-dev liblzma-dev \
     libgnatcoll-gmp19 libgnatcoll-gmp18-dev libgnatcoll18 libgnatcoll18-dev \
     libgnatcoll-doc libgnatcoll-iconv18-dev libgnatcoll-readline18-dev \
     libgnatcoll-sqlite18-dev
#
USER codespace
```

That is it from an environment setup perspective, other packages can be used instead as required. The Codespaces Terminal window can now be used to compiler *Ada* codes as would be done on system via a terminal (ie `gprbuild`, `gnatpp`, etc). The installed packages in the above *Dockerfile* for this test project are way more than is actually needed, but I just wanted to check that the most commonly used packages can be installed at this stage.


## General Usage

The *Codespaces* environment works very well &mdash; especially if you have used *Visual Studio Code* before. Once you get into the coding, I forget I am using *Codespaces* in a web browser,the environment it provides is virtually the same as a local *Visual Studio Code* editor desktop experience. 

I have used *Codespaces* from a few different browser that includes:

* **iPad Pro 12.9" with Safari** : this works fine, mostly... There is a very annoying (to me) 'feature' that means there is an additional grey bar that sometimes pops up at the bottom of the browser window that has about three icons on it - one of which is to try to hide it, the other is the on screen keyboard and undo button. I have a keyboard attached (Apple version) so I have no idea why this bar keeps appearing! It blocks the terminal windows text so you cant see the command or latest output. It is frustrating enough to stop me from using this setup.

* **iPad Pro 12.9" with iOS Edge** : I installed Microsoft Edge for iOS as a comparison. This works much better and does not have the same bottom web browser pop up bar issues that Safari does. It was very usable and works well. Edge on iOS has it's own quirks though, so I cant say I would use Edge as my main browser on the iPad, but it would certainly be preferable to use *Codespaces* over Safari.

* **Surface Go2 with Edge** : this worked very well &mdash; as you would expect. The Surface Go2 has the latest normal channel version Windows 10 and the Edge browser installed. The use of *Codespaces* in the browser window was much the same as using the desktop version of *Visual Studio Code*, accept for the browser tab and bookmarks bar being there at the top. I expect they could be hidden to provide a more immersive experience!


## Coding Experience

I just created a quick *hello world* program as a very basic test. I did most of the work in the *Codespaces* Terminal window primarily through habit &mdash; creating a new project folder, and then adding a `.gpr` project file, and a single *hello world* main `.adb` source file.

The support for coding in *Ada* was provided and worked very well using the [AdaCore - Language Support for Ada](https://marketplace.visualstudio.com/items?itemName=AdaCore.ada) from the *Visual Studio Code Marketplace* addon. The experience in the short time I used it was the same as when using it on Linux or Windows desktop versions of *Visual Studio Code*. The Ada Language Server works well, and provides the normal code completions and support for the source code files &mdash; so all good!


## Outcome

It is impressive the consistency in experience between *Codespaces* in a web browser and *Visual Studio Code* on a desktop!  The fact it works and is relatively easy to get an *Ada* development up and running in a web browser is very interesting.

Would I use it full time though..?  Not sure yet, but it is certainly worth considering as it would offer a quick to use and self contained project environment, without having to set up everything locally. For collaboration it would be interesting, as a common setup could be used by the team &mdash; but for an individual hobby developer - it could be convenient once the setup knowledge is in place...

I expect there will be a charge for using *Codespaces* once it leaves the beta testing period. How much that is, will probably drive its take up, especially for hobby programmers who don't necessarily have lots of money to throw at remote cloud based services normally.

Next I will try to use *Codespaces* with an existing project, and see how easy it is to quickly add the environment to a project, and work on it for a while. This might help solidify a view.

It would also be interesting to explore how the use of [Alire](https://alire.ada.dev/) could help with the *Ada* language support setup, maybe from a *Dockerfile* perspective, and perhaps using `alr` to pull in packages needed, so they don't all have to be provided via the local package manager. Also, I expect it is possible to script (or use *Docker*) to pull in the *AdaCore GNAT Community* version instead, if that was a preference...  Lots to think about, but as a portable and quick to use *Ada* development environment &mdash; it is possible!


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
