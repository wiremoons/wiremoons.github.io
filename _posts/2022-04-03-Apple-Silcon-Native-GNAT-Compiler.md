---
title: "Installing an Apple Silicon Native GNAT Compiler"
description: "Installing an Apple Silicon (M1) native arm64 GNAT GCC compiler with user privileges only." 
published: true
date: 2022-04-03
tags : [ "ada", "language", "ada-lang", "gnat", "compiler", "arm64", "apple silicon", "m1" ]
categories : ["development", "ada", "gnat" ]
---

# Installing an Apple Silicon Native GNAT Compiler

## Background

As most people who follow technology developments (and others too perhaps) know,
_Apple_ are in the process of moving from _Intel_ `amd64` CPU based computers,
to new _Apple Silicon M1_ `arm64` based computers. While the benefits are many
(such as more power efficient performance), there are also downside.

One of these is the initial lack of native programs compiled to run and get the
best use out of the new `arm64` M1 technology. Over the past 18 months the
situation has been improving rapidly.

One area that also obtained native `arm64` support was a _pre-release_ build of
**GCC GNAT 12.0.1 (arm64)** compiler and tools, built and shared by
[Simon Wright](https://forward-in-code.blogspot.com). Simon announced the
pre-release on his GitHub page (see below) and on the
[Ada Reddit forum](https://www.reddit.com/r/ada/comments/ttdb9m/comment/i2zdqv5/?utm_source=share&utm_medium=web2x&context=3).

This was great news to me, as I have been waiting to run _GNAT_ on my M1
machines to be able to do native Ada language development for well over a year,
so was very keen to try it out!

## Installing GCC GNAT 12.0.1 (arm64)

The download from Simon Wright's release page comes as a `.pkg` file, and when
run, installs the GNAT compiler and supporting Ada language development tools to
the folder: `/opt/gcc-12.0.1-aarch64`.

I tried this out and it all works very well - thank you Simon!

## Installation Personal Preferences

In the install notes, Simon does point out that the default install directory
can be changed post install if needed - for example just move the install
directory to a slightly shorter path if needed:

```
sudo mv /opt/gcc-12.0.1-aarch64 /opt/gcc-12.0.1
```

Apart from this, my other personal preferences for the install would be:

- don't install the files system wide, instead install into a users preferred
  directory if possible;
- install the files with least privileges needed, instead of `root:wheel`.
- don't require the installation of _Rosetta 2_ to run the package installer;

Please do not think there is anything wrong with the default installer - these
are just **personal preference**.

## The Fix To Make Me Even Happier!

Below I describe my workaround and approach to using the great work Simon has
done, and using it to satisfy my own requirements. I am only sharing them in
case others have a similar desire, or the information is useful for a similar
exercise perhaps...

The steps I followed are document below, should you wish to read on!

Fist, download the installer package '**Asset**' file from
[Simon Wright's GitHub 'Release' page](https://github.com/simonjwright/distributing-gcc/releases).

Make sure the correct _pre-release_ version is downloaded as the names are
similar, bit only the one tagged as `b9b013b` is the native _Apple Silicon_
version compiled for native `arm64` use. The direct URL is below, but check the
tag, and also for any new '_pre-released / released_' versions:

- https://github.com/simonjwright/distributing-gcc/releases/tag/aarch64-apple-darwin21-2

Once downloaded the file will probably have the macOS _quarantine_ flag enabled:
`com.apple.quarantine`. This can be checked in a _Terminal_ window by listing
the file with the command: `ls -l@`, as the '`@`' also shows the macOS extended
attributes for any file. Alternatively the `xattr` command can do the same.

The package file I downloaded is named: `gcc-12.0.1-aarch64-apple-darwin21.pkg`.

```console
% ls -l@

total 477312
drwxr-xr-x  3 simon  staff    96B  3 Apr 09:59 ./
drwxr-xr-x  8 simon  staff   256B  3 Apr 09:59 ../
-rw-r--r--@ 1 simon  staff   233M  3 Apr 09:58 gcc-12.0.1-aarch64-apple-darwin21.pkg
	com.apple.metadata:kMDItemDownloadedDate	  53B 
	com.apple.metadata:kMDItemWhereFroms	 689B 
	com.apple.quarantine	  57B
```

To remove the `com.apple.quarantine` attribute the on the downloaded file:
`gcc-12.0.1-aarch64-apple-darwin21.pkg` use the command:

```console
xattr -r -d com.apple.quarantine gcc-12.0.1-aarch64-apple-darwin21.pkg
```

or just clear all the extended attribute information from the file with the
command:

```console
xattr -c gcc-12.0.1-aarch64-apple-darwin21.pkg
```

If you are not bothered (or already have) _Rosetta 2_ installed for macOS, and
you just want to install with package installer defaults, then you can now go
ahead and install the package file by double clicking it in the macOS _Finder_,
or via a _Terminal_ with the command:

```console
installer -pkg gcc-12.0.1-aarch64-apple-darwin21.pkg -target /
```

If you wish to continue to bypass the packages installer choice, and manually
install yourself, then read on...

The actual files contained in the package installer can be installed manually to
a location of your choice. By default the package installer will install with
_administrator_ privileges on you computer, and will also install all the files
with `root:wheel` permissions, into the directory: `/opt/gcc-12.0.1-aarch64/`

My preference is to run the Ada GNAT compiler and tools with user privileges
only, so also locating them within my own home directory too.

This can be done by first extracting the files from the installer package, and
then by moving the GNAT Ada compiler and tool to a new directory in `$HOME`.

First unpack the files from the package install (ie the file
`gcc-12.0.1-aarch64-apple-darwin21.pkg`) into a new directory (called
`gcc-unpack` in the example), and then change into the new directory, and then
extract the package files into it.

_NB:_ Don't try to copy the installer package into the new unpacking directory
and then extract them, as you will see lots of messages starting with:
`error while extracting archive:...`.

Make sure you are in the same folder (ie perhaps `Downloads`) as the downloaded
file `gcc-12.0.1-aarch64-apple-darwin21.pkg`. The enter the command below into
the _Terminal_:

```console
mkdir gcc-unpack
cd gcc-unpack
xar -xf ../gcc-12.0.1-aarch64-apple-darwin21.pkg
```

Once the above commands have been completed the files and directories in the
`gcc-unpack` directory should be:

```console
% ls

Distribution                           Resources/                             gcc-12.0.1-aarch64-apple-darwin21.pkg/
```

The actual file we want is the `Payload` file located here:
`pkg-unpack/gcc-12.0.1-aarch64-apple-darwin21.pkg/Payload`

Move into that sub-directory with the command:

```console
cd gcc-12.0.1-aarch64-apple-darwin21.pkg/
```

and then extract the contents of the `Payload` file into a new `gnat` directory
(or another name of your choosing!) using the commands:

```console
mkdir gnat
cd gnat
cat ../Payload | gunzip -dc | cpio -i
```

That is the last extract step, and if the contents of the new `gnat` directory
are listed (as below) then the full compiled copy of the `gcc`, `g++` and `gnat`
etc are all available in the `bin` sub-directory. The output should be:

```console
% ls

bin/     include/ lib/     libexec/ ocaml/   python/  share/

% ls bin/

aarch64-apple-darwin21-c++*        gcc*                               gnatchop*                          gnatpp*                            gprslave*
aarch64-apple-darwin21-g++*        gcc-ar*                            gnatclean*                         gnatprep*                          lal_parse*
aarch64-apple-darwin21-gcc*        gcc-nm*                            gnatcoll_sqlite2ada*               gnatstub*                          lal_playground*
aarch64-apple-darwin21-gcc-12.0.1* gcc-ranlib*                        gnatinspect*                       gnattest*                          lto-dump*
aarch64-apple-darwin21-gcc-ar*     gcov*                              gnatkr*                            gprbuild*                          nameres*
aarch64-apple-darwin21-gcc-nm*     gcov-dump*                         gnatlink*                          gprclean*                          navigate*
aarch64-apple-darwin21-gcc-ranlib* gcov-tool*                         gnatls*                            gprconfig*
c++*                               gnat*                              gnatmake*                          gprinstall*
cpp*                               gnat_compare*                      gnatmetric*                        gprls*
g++*                               gnatbind*                          gnatname*                          gprname*
```

The final steps are to move the extracted files and directories to a location we
want to run them from, which will be under my home directory, and then add the
`bin` sub-directory to my shells path, so the tools can be found and run from
the _Terminal_. Commands used to move the files to my home directory (ie `~`)
are:

```console
cd ..
mv gnat ~/
cd ~
ls gnat/bin/
```

The files should all now be in place where you want to use them from...

To ensure the tools are available in the _Terminals_ `$PATH` add the following
to the file: `~/.zshrc` (ie assuming the default macOS `zsh` is being used!):

```shell
#-----------------------------------------------------------------------------
#    SETUP Path for local Ada GCC 12.0.1 (macOS arm64 version) installation
#-----------------------------------------------------------------------------
# add path update if Ada GCC is installed in '~/gnat`
if [[ -d $HOME/gnat/bin ]] && [[ ":$PATH:" != *":$HOME/gnat/bin:"* ]]; then
    export PATH=$HOME/gnat/bin:"${PATH}"
fi
```

If you used a different directory in your `$HOME` other than `~/gnat` to store
the _GNAT_ compiler and tools, then change the above to that directory instead.

Once you re-start the _Terminal_ it should pick up the installed _GNAT_ compiler
and tools in your PATH, and you can now use them.

The download files and other extracted files _left overs_ from the package
installer can now be deleted as well if you wish.

Next - enjoy, and do some Ada development natively on your Mac.

## Future Wishes

Hopefully, in the near future the _GCC GNAT compiler and tools_ (or even the
[GNAT LLVM](https://github.com/AdaCore/gnat-llvm) equivalent!) will be
installable via [Homebrew](https://brew.sh/) or using the
[Alire](https://alire.ada.dev/) and its great supporting tool `alr`.

## Learning More About Ada

There are plenty of good resources for more background on the Ada language such
as:

- [Wikipedia Ada Language page](https://en.wikipedia.org/wiki/Ada_(programming_language))
- [AdaCore - resources page](https://www.adacore.com/resources)

One great free resource to learn the Ada language from
[AdaCore](https://www.adacore.com/), so you can write your own programs, is
available here:

- [Learn Ada Course](https://learn.adacore.com/courses/intro-to-ada/index.html)

You can also download a very nice PDF book copy of the above learning Ada
resource, that was recently updated (ie _Release 2022-03_):

- [Learn Ada Course (PDF)](https://learn.adacore.com/pdf_books/courses/intro-to-ada.pdf)

### Article Details

- Title: _{{ page.title }}_
- Published Date : {{ page.date | date: "%d %b %Y" }}
