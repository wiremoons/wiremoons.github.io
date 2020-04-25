---
title: "Nim Debugging Approaches"
description: "A few different ways to manage debugging output in the Nim programming language"
published: true
date: 2020-04-18
tags : [ "nim", "debug", "debugging", "nim-lang", "python", "template" ]
categories : ["development", "debugging", "nim" ]
---
# Nim Debugging Approaches

The article explores debugging [Nim](https://nim-lang.org/) programs, and different approaches to outputting debug messages from a simple example program. All the included code was tested with Nim compiler version 1.2.0 running on Linux ARM.

### Approaches to Debugging Source Code

Whatever programming language is being used, one requirement that pretty much exists in all of them, is the ability to carryout debugging. Nim, while being a concise and no fuss language with easy to read and very Python like source code syntax, is no different.

When you have an error in a program being written, or just want to check a bit of code is working as expected, or to trouble shoot a breakage - some method is needed to provide a level of extra detailed information when the program runs.

The best approach is to use an actual debugger most of time, such as [GDB](https://www.gnu.org/software/gdb/), to step through the program and view everything you need. Sometimes this is overkill, and all you want is for you program to print out a message to say it has reached a certain point when running, or tell you what a variable currently contains. 

Of course just printing out messages is often the simplest approach, and easiest too. It does mean that the source code can get littered with `echo "debug message"` or even `write(stderr, "debug message\n")`. Once the program is complete, unless they are removed one by one, these extra code snippets of course end up being compiled into the program, even though it is no longer being debugged!

So what are the approaches that can be used?


### A Very Simple Nim Program

To demonstrate some different approaches, a very simple (and boring) example program in Nim is needed:

```nim
# file called 'myprog.nim'

proc messageOut(myMsg:string) =
  echo "My message is: " & myMsg
  
# runs from here
messageOut("Hello from Nim!")

```
The above example code can be saved in a text file named: `myprog.nim` and then can be compiled with the command: `nim c myprog.nim`. The compiled program can be run as `./myprog` or on Windows with `myprog.exe`. All it does is print out: `My message is: Hello from Nim!`.


### Adding Basic Debug Output

So lets add some extra output to help debug the program, to see what it is doing as it is run:

```nim
# file called 'myprog.nim'

proc messageOut(myMsg: string) =
  echo "DEBUG: in proc 'messageOut' with myMsg: '" & myMsg & "'."
  echo "My message is: ", myMsg

# runs from here
echo "DEBUG: program is running!"
messageOut("Hello from Nim!")
echo "DEBUG: program is done"

```

The above works when compiled in the same way, and we can now track what the program is doing as it runs! 

But this source code looks very messy, and of course that extra debug code is always going to be printing all that extra 'DEBUG' output information whenever it runs. Not a very good solution, except for a very quick temporary debug perhaps...


### Controlling the Kind of Compiled Program

So what other approach could be used? Well Nim is clever in that when it compiles the program, you can tell it if you want a '*debug*' or '*release*' version of the program. This can be done as extra flags to the compile command:

```bash
# for a 'debug' version (the default):
nim c myprog.nim     becomes:     nim c -d:debug myprog.nim

# for a 'release' version:
nim c myprog.nim     becomes:     nim c -d:release myprog.nim
```

The advantage of this approach is that we can now better manage the debug messages in to programs code, so it will only display the additional debug messages, when it is compiled as a '*debug*' version of the program.

This can then be used by our program, by adding an extra check in the source code. This check is used to see what was requested when the program compiled - so that when it is run, it can be decided if the extra debug output should be displayed or not. The updated source code file adds this additional check with: `when not defined(release): ` as shown below:

```nim
# file called 'myprog.nim'

proc messageOut(myMsg:string) =
  when not defined(release): echo "DEBUG: in proc 'messageOut' with myMsg: '" & myMsg & "'."
  echo "My message is: ",myMsg
  
# runs from here
when not defined(release): echo "DEBUG: program is running!"
messageOut("Hello from Nim!")
when not defined(release): echo "DEBUG: program is done"

```

If the above is compiled again, but this time also specifying it is either a '*release*' of '*debug*' version, the debug extra output will only appear when it is specifically compiled as a '*debug*' version of the program. 

Well that's an improvement, but it is still quite a lot of extra code to sprinkled throughout the programs source code. So the next improvement is to use a **Nim '*template*'**!


### Using a Nim Template to Manage Debug Output

In the main source code file, just above the existing `proc messageOut()` code, a Nim *template* has been added (see below). This simplifies the output of any debug information that needs to be displayed. It creates the ability to use `debug` in place of our previous `echo` command as used before. It will still only output any debug messages when the program is compiled in '*debug*' mode. So we don't loose any of the previous functionally, just gain a simplification, and cleaner looking, with easier to understand code!

```nim
# file called 'myprog.nim'

# template added
template debug(data: untyped) = 
  when not defined(release):
    echo data

# source below updated to use 'debug' template
proc messageOut(myMsg:string) =
  debug "DEBUG: in proc 'messageOut' with myMsg: '",myMsg,"'."
  echo "My message is: ",myMsg
  
# runs from here
debug "DEBUG: program is running!"
messageOut("Hello from Nim!")
debug "DEBUG: program is done"

```
The program is compiled and run as before, and if compiled as a '*debug*' program (ie not '*release*'), then as before, it outputs the additional information. The source code looks a lot cleaner, and is easier to understand overall, once it is understood what the extra '*template*' part of the code is for!

### Debugging For Multiple Source Code Files in Projects 

While this is fine for our very simple program with one source code file, what if we had a project with lots of different source code files, and we want to use the new `debug` feature in all of them?

Well, the '*template*' can be put into its own source code file as a Nim '*module*', and then just imported into any of the other source code file(s) in our project, that may want to use it. To convert our small program ready for this approach, we create two source code files now, as below.

The first one just holds our '*template*' `debug` output code - but note: **it is important to add a '`*`' after the name of the template name - in our case after the word `debug`**. This tells the Nim compiler that this *template* code can be used by other source code files in our project. This allows the actual main program code to call the debug *template*. Without this additional '`*`' this wont work!

```nim
# file called 'debugUtils.nim'

template debug*(data: untyped) =
  when not defined(release):
    echo data
```

The main source code file remains much as before - except there is an `import debugUtils` statement, which allows the '*template*' code to be used. This same approach can be repeated across many other source code files that our project may have, which wish to also use the `debug` output capability too! 

Also note that the '*templates*' source code file name suffix (ie `.nim`) is not included in the `import debugUtils` statement. If the `.nim` part is included the compiler will report an error and not work. The complete main program source code file is now as below:

```nim
# file called 'myprog.nim'

# import the debug code template: 'debugUtils.nim'
import debugUtils


proc messageOut(myMsg: string) =
  debug "DEBUG: in proc 'messageOut' with myMsg: '" & myMsg & "'."
  echo "My message is: ", myMsg

# runs from here
debug "DEBUG: program is running!"
messageOut("Hello from Nim!")
debug "DEBUG: program is done"

```

The program is still compiled as before: `nim c myprog.nim`, and the compiler is clever enough to include the additional `debugUtils.nim` source coded file on its own. The program should run as before once compiled.

This final '*template*' option is much cleaner, more versatile, and flexible, and saves a lot of repeated code in a project. It also allows one file to be changed that affects all the other files that use the `debug` facility. The '*template*' code can be adjusted if need, without having to find every `debug` output statement in all the source code files that a project may have. 


### Debug Messages As Errors

So far, the '*DEBUG*' messages we have added have been displayed when the program runs, and are mixed in with programs normal output.
While this works well, it is better to allow the debug messages to still be displayed, but also allow them to be managed separately if needed. It is good practice to print debug messages as '*error*' outputs - not a part of the normal programs output. So to achieve this separation, the Nim '*template*' can be adjusted to send it output to '**stderr**' an abbreviation for '*standard error*'. Normal program output is sent to '**stdout**', which is an abbreviation for '*standard output*'. Standard output is is what the Nim `echo` procedure uses.

To send the '*debug*' output to '*stderr*' instead, we need to switch from `echo` to `write`, that provides this extra capability.

Another improvement is that any debug output can be prefixed with '*DEBUG*' by the '*template*' - so removing the need to add this extra text in our main programs source code files over and over again! The '*template*' (see below) now adds the '*DEBUG*' text before the actual debug message, and then also appends a newline character (ie `\n`) to the end, so we start a new display line after the debug message is output.


```nim
# file called 'debugUtils.nim'

template debug*(data: untyped) =
  when not defined(release):
    write(stderr, "DEBUG: " & data & "\n")
```

The main program source code is as before, just the 'DEBUG' text has been removed from the debug messages, as that is added now automatically by the '*template*' instead.

```nim
# file called 'myprog.nim'

# import the debug code template: 'debugUtils.nim'
import debugUtils


proc messageOut(myMsg: string) =
  debug "in proc 'messageOut' with myMsg: '" & myMsg & "'."
  echo "My message is: ", myMsg

# runs from here
debug "program is running!"
messageOut("Hello from Nim!")
debug "program is done"

```

That's it - just compile and run as before, and the output should be the same as before too. Now however, the program can also be run with additional parameters: `./myprog 2> debug.txt`.

When run in this way, the debug messages no longer get shown on screen, and just the programs normal output is seen - much the same as when it is compiled as a '*release*' version of the program.

What the extra ` 2> debug.txt` part does is redirect any error output into a new file called `debug.txt`. If you view this file in a text editor (or: `cat ./debug.txt`) it will show all the debug messages it captured when the program ran. This is a nice way to separate out the debug messages being sent to '*stderr*' and the normal programs output being sent to '*stdout*'.

Hopefully this ability to manage debug output in an Nim program is useful, and helps to keep the main source code neat and clean, much like Nim source code is generally supposed to be!


### Ensuring Extra Debug Information is Included for GDB

Of course, this does not replace in depth debugging to investigate a `SEG FAULT` perhaps, but it is certainly very useful, and often all that is needed in everyday coding with Nim. Keep it simple!

If you do plan to use a debugger such as `gdb` with your Nim program, then it is worth ensuring Nim compiles as much debug support information into you program as it can. The additional Nim compiler flags that can be used to help achieve this are as follows:

```nim
nim c -d:debug --lineDir:on --debuginfo --debugger:native myprog.nim
```

If you want to find out more about Nim, visit the great web site pages here: [Nim language site](https://nim-lang.org/) or if you need more help, the [Nim Forum](https://forum.nim-lang.org) is friendly and supportive community location to call on.


### Article Details

- Title: {{ page.title }}
- Published Date : {{ page.date | date: "%d %b %Y" }}
