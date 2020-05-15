---
title: "Nim 'fmt' for Version Output"
description: "Using the Nim programming language 'fmt' proc for outputting an applications version information"
published: true
date: 2020-05-03
tags : [ "nim", "fmt", "text display", "version", "cli", "nim-lang", "python", "template" ]
categories : ["development", "text", "nim" ]
---
# Nim 'fmt' and Version Display

Having programmed with the [Go language](https://golang.org/) previously, one of the features I liked was being able to quickly construct a text screen for output. This also included the ability to have variables from the program display information when it was run. Using this text template generated output method, was a quick way to build a screen of information, that is dynamically completed, and then output when the program runs.

I was pleasantly surprised and pleased to discover the same great feature is a available for use with the language [Nim](https://nim-lang.org/).

This article looks at the use of the Nim `fmt` proc, and then how I have used it in a few programs I have written to output the programs version information. All the included code was tested with [Nim compiler](https://github.com/nim-lang/Nim) version 1.2.0 running on a Raspberry Pi 4B, so Linux ARM. 

Of course, as it is Nim code, it can also be used on other flavours of Linux, xBSD, Windows and macOS too, without any additional effort!

### So What is this 'fmt' proc?

Include in the fantastic '*batteries included*' Nim standard library (ie so it comes with the normal Nim complier installation) is a module called: `strformat`. A Nim module is similar to a '*library*' from other languages, so provides commonly used functionality for the language, to save you from having to write it yourself!

The [Nim documentation page](https://nim-lang.org/docs/strformat.html) states the `strformat` module is to provide:

> String interpolation / format inspired by Python's f-strings.

I can't remember if I ever used the [Python language](https://www.python.org/) equivalent, as it was many years ago since I touched Python. But that is good, because while Python maybe slower generally and need more *baggage* runtime wise than Nim or Go, it is however a good and productive language to program in.

So before looking at the `fmt` proc - what am I using it for exactly..?

### Long String Literals

It is very common to want to output a string in a language, easily done in Nim:

```nim
# create a simple string called 'mystring' 
let mystring = "This is string of text"
# output the string of text 
echo mystring
```

It is also a not so common requirement to want to output a block of text from a program, and this can also be done using a '*long string literal*' as below:

```nim
# create a long string literals called: 'myStrLiteral'
let myStrLiteral = """
  A longer string - perhaps over a 
     couple of lines which should be output as it
  is seen here, keeping the layout etc!
"""
# now output it
echo myStrLiteral
```

So, as shown, this may be very useful if a program needs to output a block of text that doesn't change whenever it is run.  But, what if the program wanted to output a block of text and include an additional bit of information, that changed whenever it was run?

### Concatenate A String

Easy right &mdash; Nim has some really good support for string manipulation, so we can just build up a string of output first. The string we need to output can be joined together using concatenation, or just use `echo` lots of times until the screen of output is shown...  May be something like:

```nim
# create a string variable that needs to be output:
var mystring = "*include me too!*"
mystring.add(" - plus add this bit")
# now output the text we need
echo ""
echo "This program is to output"
echo "   lots of interesting text"
echo "that must also ", mystring, " when output"
echo "    perhaps too much 'echo' usage...."
```

This works fine, and will output the following when run:

```
This program is to output
   lots of interesting text
that must also *include me too!* - plus add this bit when output
    perhaps too much 'echo' usage....
```

But even for a small example it is a lot of repetitive code, such as the us of `echo`. While it could be simplified, and the whole output string could be constructed first a then output... perhaps there is a better way?

### Using Long String Literals and 'fmt'

So, this is where the `fmt` proc comes in to play. 

It can allow the programs *variables* and *long string literals* to be combined together for a screen of output.

To use the `fmt` proc (as it is not on the `system` module that Nim imports by default) we need to add it to the program to use it. This is traditionally done at the start of the program with the key word `import`. 

The string literal to be used then is sent to the `fmt` proc so it can work its magic, and then the whole constructed output is sent to the screen by `echo`. 

**Note:** that any *variables* that are to be included and positioned in the output by `fmt` are placed in the text &mdash; but surrounded by curly braces (ie `{` and `}`). A simple example would look as follows:

```nim
# make sure the 'strformat' module is available to our program
import strformat

# create two string variables that needs to be output:
let mystring1 = "*include me too!*"
let mystring2 = " - plus add this bit"

# now output the text we need
echo fmt"""
This program is to output
   lots of interesting text
that must also {mystring1} when output, {mystring2}
    and not as much 'echo' usage now.
"""
```

When run the output displayed is as follows:

```
This program is to output
   lots of interesting text
that must also *include me too!* when output,  - plus add this bit
    and not as much 'echo' usage now.

```

I think it that is a much better solution, and as the needed program output starts to grow or become more complex, the simpler it is to use, and the easier it should be to manage and debug if needed.

Also, the above just starts to touch on what `fmt` can be used for! 

It can also control the output of floating point numbers, so they only display two decimal places for example; or run and display other procedures output too:

```nim
# make sure the additional 'strformat' and 'times' modules are 
# available to our program
import strformat, times

# create a long floating point number, and then manage its output:
let longFloat = 123.456789
echo fmt"Number '{longFloat}' as two decimal places: {longFloat:0.2f}"

# run other procs inside the 'fmt' parameters
echo fmt"""

   -> The date today is      :  {getDateStr(now())}
   -> The time currently is  :  {getClockStr(now())}
 """

```

Will output as below, but with the date and time from when the program actually run:

```
Number '123.456789' as two decimal places: 123.46

   -> The date today is      :  2020-05-03
   -> The time currently is  :  11:39:23
 
```

### Back to a Program 'Version' Output

So back to the point of this article. 

I wanted a way to output some information from my program that includes some fixed information, but also some information generated at runtime, and compile time too. 

Most programs include a simple output to tell you what version they are, so you can check if you are running the latest version available, or perhaps to check it is not the version you heard has a bug...

A version output screen is quite a common requirement, so for my programs I decided I would create a proc called: `showVersion()` that is included in the example below.

The code below was saved into a file called: `myApp.nim` and then complied and run with the command: `nim c --run myApp.nim`

```nim
import strformat, os

proc showVersion() =
  # check if compiled as a 'debug' or a 'release' version
  const verKind = when defined(release): "release" else: "debug"
  # obtain the Nim complier version
  const buildV = fmt"Build is: {verKind} using Nim compiler version: {NimVersion}"
  # get the name of this running program
  let appName = extractFilename(getAppFilename())

  echo fmt"""

'{appName}' is version: '1.0.1' running on '{hostOS}' ({hostCPU}).
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
The output the program creates when run is as follows:

```
The program is running now...

'myApp' is version: '1.0.1' running on 'linux' (arm).
Copyright (c) 2020 Wiremoons Blog.

Compiled on: 2020-05-03 @ 12:02:04.
Build is: debug using Nim compiler version: 1.2.0.

For licenses and further information visit:
   - myApp application : https://github.com/wiremoons/myApp/
   - Nim language & compiler : https://github.com/nim-lang/Nim/

```

### What Next?

Well hopefully that shows a useful example of how the `fmt` procedure can be used. The above `showVersion()` could be used as a basis to create a *version output* module, which could then be re-used for different Nim programs, that would only need minimal changes...

Putting the `showVersion()` procedure in a separate module, also ensures the code doesn't cluttering up any main code file. However, if the procedure is moved to its own module, it would also need to be renamed as: `showVersion*()` to ensure it can be called from a different source file (ie module).

If you want to find out more about Nim, visit the great web site pages here: [Nim language site](https://nim-lang.org/) or if you need more help, the [Nim Forum](https://forum.nim-lang.org) is friendly and supportive community location to call on.


### Article Details

- Title: *{{ page.title }}*
- Published Date : {{ page.date | date: "%d %b %Y" }}
