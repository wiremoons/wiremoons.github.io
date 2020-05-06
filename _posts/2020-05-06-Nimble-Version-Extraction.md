---
title: "Nimble Version Extraction"
description: "Using Nimble for including an applications version in source code"
published: true
date: 2020-05-06
tags : [ "nim", "nimble", "text display", "version", "cli", "nim-lang", "python", "template" ]
categories : ["development", "text", "nim", "nimble" ]
---
# Nimble Version Extraction

In a previous article, the `fmt` command was used to display a text output that provided the applications current version information. That article is called: [Nim ‘fmt’ and Version Display](http://www.wiremoons.com/development/text/nim/2020/05/03/Nim-Fmt-for-Version-Output.html) in case you have not read it, and want to have a look. The article below does refer back to that one, as they are related from a progression perspective.

This article will look at a Nimble feature that allows the inclusion of the Nimble configured version into the application source code, automatically.


### So what is Nimble?

Shortly after writing the above linked article I noticed that a new version of [Nimble](https://nimble.directory/) had been released. Nimble is described as:

> Nimble is a beta-grade package manager for the Nim programming language.

The '*beta-grade*' statement should not put anyone off!! It works very well, and I have not had any issues using at all. It is an extremely useful tool that can be used to:

- search for, install, update, and remove Nim programs and packages;
- to assist with setting up a new Nim project;
- manage the inclusion of, and community sharing of Nim packages in the Nimble directory;
- manage the build of a Nim programs source code, including a powerful *task* Nim script facility.

I expect it does a lot more &mdash; and it certainly does a good job (if not better!) than similar package managers used for other languages such as: [opam](https://opam.ocaml.org/) for OCaml, [cargo](https://crates.io/) for Rust, [go get](https://pkg.go.dev/) for Go (Golang), or even [npm](https://www.npmjs.com/) for Node.js and Javascript.


### So Whats New in Nimble...

So, looking through the [Nimble GitHub](https://github.com/nim-lang/nimble) pages at the [latest released notes / changelog file](https://github.com/nim-lang/nimble/blob/master/changelog.markdown#0112---02052020) to find out what was new, I came across a nice new feature for including an applications version from its Nimble configuration file.

This *new* feature is not that new as it was part of the Nimble release **0.11.0** dated 22 September 2019. It is new to me in May 2020 though!

I had looked for this capability before, having used similar build features in other languages. There were text parsing examples about, but no simple method existed that I could find. However, it was still something I thought would be a good feature, but I had many other things to learn about Nim and Nimble before I disappeared down another internet rabbit hole.

### What is interesting and useful about this feature

When Nimble is used to manage a Nim project, the `nimble init` command helpfully walks the developer through creating a configuration file for their Nim project. This file contains the applications dependencies, author details, license, version information, Nim compiler version dependency, and the programs current version.

Unfortunately, until version `0.11.0`, that Nimble configuration version information was not easily accessible to be included in the programs source code too. This meant when the program had its version incremented, the Nimble configuration file, the version in the source code file, and the Git (or other source code management tool), all individually had to be updated, and be kept in sync.

This new Nimble feature will reduce that list of updates to just the Nimble configuration file, and the source code repo versioning, such as using Git tags.

As ever with Nim related support, the documentation was very helpful, and there was a link included in the release notes to an [example](https://github.com/nim-lang/nimble/blob/4a2aaa07d/tests/nimbleVersionDefine/src/nimbleVersionDefine.nim) of how to use this new (to me) feature! Just what a beginner Nim user needs to keep them on the move with their learning experience and fun. 

The example shows:

```nim
when isMainModule:
  const NimblePkgVersion {.strdefine.} = "Unknown"
  echo(NimblePkgVersion)
```

The important line of code I needed was: `const NimblePkgVersion {.strdefine.} = "Unknown"`. 

As this variable is defined as a `const` that means the variable is created at compile time, not when the program is running. So I am guessing some sort of Nim macro is being used to extract the Nimble version information from the Nimble configuration file during compilation. I would go and browse the source code to find out, but this article would then never see the light of day I expect, and I would be off on another tangent adventure instead.

I am not entirely sure (yet) what the `{.strdefine.}` part does, but my guess is that should the Nimble configuration file version information not be found at compile time, then the variable `NimblePkgVersion` is set to the string `"Unknown"` so it does not error out...  I am sure I will discover more about these weird looking statements in code, as my Nim adventures continue!


### So how can this be used?

A version output screen is quite a common requirement, so for my programs, and as documented in a prior article (see the intro to this one for a link), I decided I would create a proc called: `showVersion()` that is included in the example code below.

As can be seen in that prior article, this works well, but the application version is manually entered as: '`1.0.1` in the source code. 

Using the new Nimble feature this can be automated further. 

When the updated code below is compiled and run without Nimble, the output shows `Unknown` for the application version. So at least the compile does not break if Nimble is not being used for some reason.

This time the same code will be used as before, but the Nimble version version extraction code will be added, and the program will be managed with Nimble, instead of just compiling it directly with `nim c --run myApp.nim`. The steps taken to use Nimble for the project are as follows.

**Note:** Nimble will create a new sub directory for your project when it is run in the way outlined below. So before you start to run the commands shown, make sure you are in a terminal and located in a suitable directory to store the Nim code and project in. The `nimble` program should be installed on you computer already if you have a current version of Nim installed, as it comes with the compiler and other useful tool by default. See the [Nim language site](https://nim-lang.org/) pages for more help on installing Nim first, if you need too! If you need to update Nimble to the latest version, then running: `nimble install nimble` should work.

### Initialise a project with 'nimble'

To use Nimble to manage and build the program first initialise it by running the command: `nimble init myApp`.  

Normally the `myApp` part would be your choice of project name. But if you want to follow along, I suggest the same name is used &mdash; to help with understanding the outputs you will see, against those shown below.

You will be prompted to answer a set up questions, so select the preferred answer using the `TAB` key, or type values in, and then press return. My input was as follows:

```
      Info: Package initialisation requires info which could not be inferred.
        ... Default values are shown in square brackets, press
        ... enter to use them.
      Using "version_demo" for new package name
      Using Wiremoons Blog for new package author
      Using "src" for new package source directory

    Prompt: Package type?
        ... Library - provides functionality for other packages.
        ... Binary  - produces an executable for the end-user.
        ... Hybrid  - combination of library and binary
        ... For more information see https://goo.gl/cm2RX5
     Select Cycle with 'Tab', 'Enter' when done
    Answer: binary
    
    Prompt: Initial version of package? [0.1.0]
    Answer: 1.0.2
    
    Prompt: Package description? [A new awesome nimble package]
    Answer: My test package for version display
    
    Prompt: Package License?
        ... This should ideally be a valid SPDX identifier. See https://spdx.org/licenses/.
     Select Cycle with 'Tab', 'Enter' when done
    Answer: MIT
    
    Prompt: Package Backend?
        ... c    - Compile using C backend.
        ... cpp  - Compile using C++ backend.
        ... objc - Compile using Objective-C backend.
        ... js   - Compile using JavaScript backend.
     Select Cycle with 'Tab', 'Enter' when done
    Answer: c
    
    Prompt: Lowest supported Nim version? [1.2.0]
    Answer: <just pressed return for default>
   
   Success: Package version_demo created successfully
```

Once the above questions have been answered, if you copied the same names as I used for the program name, then a new sub directory should now of been created called: **myApp**. This will contain the Nimble configuration file called: **myApp.nimble**, and also another sub directory called **src**, with a *ready to use* source code file called: **myApp.nim**. This looks as follows:

```
myApp/                       <-- new project Nim / Nimble directory
├── myApp.nimble             <-- the Nimble configuration file
└── src                      <-- the new source code sub directory
    └── myApp.nim            <-- the new 'ready to use' project Nim source file

1 directory, 2 files
```

If the Nimble configuration '**myApp.nimble**' file contents are viewed, it will look similar to the following:

```nim
# Package

version       = "1.0.2"
author        = "Wiremoons Blog"
description   = "My test package for version display"
license       = "MIT"
srcDir        = "src"
bin           = @["myApp"]

# Dependencies

requires "nim >= 1.2.0"
```

The replacement code below now includes the new Nimble version feature too, as outlined earlier. Save this code into the '*ready to use*' source code file in the `src` sub directory file called: `myApp.nim`. The '*ready to use*' example code can just be deleted and replaced with that shown below:

```nim
import strformat, os

proc showVersion() =
  # check if compiled as a 'debug' or a 'release' version
  const verKind = when defined(release): "release" else: "debug"
  # obtain the Nim complier version
  const buildV = fmt"Build is: {verKind} using Nim compiler version: {NimVersion}"
  # get the Nimble package version
  const NimblePkgVersion {.strdefine.} = "Unknown"
  # get the name of this running program
  let appName = extractFilename(getAppFilename())

  echo fmt"""

'{appName}' is version: '{NimblePkgVersion}' running on '{hostOS}' ({hostCPU}).
Copyright (c) 2020 Wiremoons Blog.

Compiled on: {CompileDate} @ {CompileTime}.
{buildV}.

For licenses and further information visit:
   - {appName} application : https://github.com/wiremoons/{appName}/
   - Nim language & compiler : https://github.com/nim-lang/Nim/
"""
#################################################################
# program runs from here:
#################################################################
echo "The program is running now..."
# call the procedure to display the version information
showVersion()
```

Now Nimble is configured and ready to be used, and the source code above should be saved in the `myApp.nim` source code file in the `src` sub directory, it should be ready to go!

From within the projects main directory run the command: `nimble run`.

The program should now runs, and the displayed output &mdash; basically the same as the last article showed. The difference is the version for the application is now being picked up from the Nimble configuration file instead:

```
The program is running now...

'myApp' is version: '1.0.2' running on 'linux' (amd64).
Copyright (c) 2020 Wiremoons Blog.

Compiled on: 2020-05-06 @ 10:06:00.
Build is: debug using Nim compiler version: 1.2.0.

For licenses and further information visit:
   - myApp application : https://github.com/wiremoons/myApp/
   - Nim language & compiler : https://github.com/nim-lang/Nim/
```

Note, that is you try the above `nimble run` command in a different directory such as the `src` sub directory, you will get an error similar to: `Error: Specified directory (/home/blog/projects/nim/myApp/src) does not contain a .nimble file.`). Just re-run the command in the root project directory where the **myApp.nimble** file is located.

This is just the start of what Nimble can do to help with developing programs in Nim. It is worth learning to use the Nimble package manager as it can save a lot of time and frustration in the long run. It also provides easy access to some very useful modules and programs written by other Nim developers.


### What Next?

Nimble can be configured to run other **tasks**, and has other commands such as: `nimble build`. These are worth reading up on and using to help with Nim projects. See the [Nimble GitHub](https://github.com/nim-lang/nimble) for more information.

If you want to find out more about Nim, visit the great web site pages here: [Nim language site](https://nim-lang.org/) or if you need more help, the [Nim Forum](https://forum.nim-lang.org) is friendly and supportive community location to call on.


### Article Details

- Title: *{{ page.title }}*
- Published Date : {{ page.date | date: "%d %b %Y" }}
