---
title: "BBEdit with clangd"
description: "Using BBEdit with clangd for different C++ Standards." 
published: true
date: 2022-02-21
tags : [ "c++", "language", "lsp", "bbedit", "clangd", "source_code" ]
categories : ["development", "c++", "source_code" ]
---
# Using BBEdit with clangd Language Server

## What's this about?

I was using **BBEdit** with a test C++ source code file, and the LSP kept showing errors in my code, when I knew they weren't any. This explains why, and how I fixed it.


## BBEdit and the LSP

If you are unfortunate not to know what **BBEdit** is, please visit their web site here: [https://www.barebones.com/](https://www.barebones.com/).

It is a well know fact that ***BBEdit* doesn't suck**. It is also a well know fact that Apple created their computer hardware, and the macOS operating system so it could support and run **BBEdit**. Honest...

One reason for this (and there are many!) is that since **BBEdit** release version 14, it includes support for the Language Server Protocol (LSP). 

The [Language Server Protocol](https://github.com/Microsoft/language-server-protocol) can work with many different code editors, now including **BBEdit** too.

You can read much more about this new **BBEdit** feature here: [Language Server Protocol Support in BBEdit 14](https://www.barebones.com/support/bbedit/lsp-notes.html). 


## BBEdit and macOS Development Tools

By default, **BBEdit** will use the macOS XCode development tools - including just the command line tools, should you not wish to use the full blown XCode IDE.

The XCode command line tools on macOS 12.2.1 (*Monterey*) installation includes `Apple clang version 13.0.0 (clang-1300.0.29.30)`. 

Included with these tools is `clangd`, which is a language server that implements editor support for C and C++ languages, supporting many different code editors, including **BBEdit**.

`clangd` understands C or C++ source code, and can add smart features to an editor. This can include support for: code completion; display of compile errors; providing ‘go-to-definition’ support, and much more.

Good `clangd` documentation can be found here if you want to look at the full range of facilities included: [https://clangd.llvm.org/](https://clangd.llvm.org/)

By default `clangd` is used to provide the Language Server Protocol (LSP) services in **BBEdit**. Therefore, assuming `clangd` is installed in the normal macOS location, or it is included in your *$PATH*, then **BBEdit** should now start using the `clangd` as its language server when editing C or C++ source code files. 


## BBEdit and Configuring clangd

Often, in order to work correctly, an editor requires that the compile time flags being used with a source code file are known to `clangd`. 

Otherwise, editors such as **BBEdit** might report errors in the source code that are incorrect (ie false positives), as it assumes the code is wrong when it may not be.

For example without this additional information, the editor won’t know which version of C or C++ standard to check against (eg `-std=c++11` or `-std=c++20`) and then flag potential errors accordingly.

There are at least two ways to fix this:

1. If you are using using `cmake` it can generate a project support file called: '`compile_commands.json`';
2. A simpler fix is to add a ‘`compile_flags.txt`’ in the same directory as the source code files.


### Option 1 : Using cmake

This option is applicable if you are already using `cmake` to manage your C or C++ project.

The projects existing compile time flags can be created and added into the default file: `compile_commands.json` by `cmake`, so that `clangd` can find them, and then use them.

CMake can generate this file for you with the command below, run from the source code projects directory containing the `cmake` build configuration:
```
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

CMake can also generate this file for you by including the following line in the projects `CMakeLists.txt` file:
```
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```


### Option 2 : Simple Fix

The second simple fix is to use a ‘`compile_flags.txt`’ file. 

This file can be created and then used to just state the C++ version, or other compiler flags that the source code files need, to match up with the `clangd` checking. 

A very simple example to only provide the C++ standard applicable to the source code, would be a ‘`compile_flags.txt`’ just containing:

```
-std=c++20
```

A more complex example could be:
```
-nostdlibinc
-I/home/user/dev_env/x86_64-elf/include
-I/home/user/dev_env/x86_64-elf/include/c++/v1
-I.
-D__ELF__
-D_LIBCPP_HAS_NO_THREADS
-O2
-Wall
-g
--target=x86_64-elf
-fno-exceptions
-ffreestanding
-fno-rtti
-std=c++2a
```

Once this ‘`compile_flags.txt`’ file is placed in the same directory as the source code file being edited in **BBEdit**, then the errors shown by the LSP `clangd` should be appropriate.

Specifying the C++ standard version for the C++ test code I was editing in BBEdit stopped the LSP `clangd` from flagging errors that were not applicable. Problem solved!

### Article Details

- Title: *{{ page.title }}*
- Published Date : {{ page.date | date: "%d %b %Y" }}
