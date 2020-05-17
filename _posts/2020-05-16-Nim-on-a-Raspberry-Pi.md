---
title: "Nim on a Raspberry Pi"
description: "Using Nim for development on a Raspberry Pi 4B"
published: true
date: 2020-05-16
tags : [ "nim", "rpi", "raspberry pi", "install", "cli", "nim-lang", "python", "compile" ]
categories : ["development", "rpi", "nim", "install" ]
---
# Nim on a Raspberry Pi

When the [Raspberry Pi 4B](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) 
came out, something about it peaked my interest... not 
sure what, as I was certainly aware of the many previous Raspberry Pi models. 
I always knew about them as the more recent models are manufactured not far 
from where I live in Wales, so there was a fair bit of news around them from that 
point of view as well.

It runs Linux by default, using a distro called [Raspbian](https://www.raspbian.org), 
which is based on Debian. The little computer looked powerful enough to be worth 
trying out, so I decided to buy one (without much of an expectation) and see 
what the fuss was about. After a little bit of research I placed an order with 
[The Pi Hut](https://thepihut.com/). The Raspberry Pi (RPI) soon turned up in the 
post, and my next hobby commenced... a year or so later, and now I have three of them!

After trying out various development approaches (perhaps a topic of another post) 
the [Nim](https://nim-lang.org) programming language caught my interest by accident.

This language has the best of everything in my view, and allows me to quickly 
build my ideas and projects, using a language that is very similar to Python. The 
compiled output ends up being very performant, and the source code is easy to maintain, 
mainly due to its simplicity and clean presentation. Should you write a program 
you want to use beyond a Raspberry Pi, Nim supports all major 
operating systems and different CPU architectures, plus the compiled program 
results in a small mostly static binary. 

Nim is certainly a language you should encourage your friends to use!


### Why Nim on a Raspberry Pi?

The Raspberry Pi 4B, is a Single Board Computer (SBC) with a four core 1.5Ghz 
ARM processor, and a choice of 2Gb or 4Gb of RAM. It has a collection of interfaces 
and ports including WiFi, Bluetooth, USB2, USB3 and Gbit Ethernet. 
It is therefore certainly a reasonable machine, can be used as a desktop, but 
is not in the category of a speedy well equipped desktop. But that's not the point.

To get the best out of the computer, having fast, compiled, binary programs 
is going to provide the most efficiency. This approach will offer a better use of the 
computers resources, especially when compared generally to using an interpreted 
language such as Python, Node.js, etc. 

Choosing a language that also compiles to small binary files sizes is better too, 
so unlike Go (Golang) for example, which produces quite large compiled binaries in 
comparison to Nim. When the SDCard based storage is slow (compared to 
a SSD) loading smaller programs into memory when they run will be beneficial too!

Java (and similar such as Clojure, Scala, Kotlin, etc) are nice languages, but while 
the Java Virtual Machine (JVM) is very fast once running, it does have a load or 
start up time delay &mdash; which if you are programming system tools and utilities 
is not helpful. Java can also consume quite a bit of a computers memory, which 
can sometimes be an issue in a resource constrained computer.

Of course Rust and C++ are efficient and fast &mdash; but personally I find them 
more complex and consequently they may have slower development times than Nim 
for example. 

When computing and development is a hobby, having a language such as Nim that 
offers quick, clean and performant results, it is always nice bonus!

Nim offers the best of lot of areas of consideration. Anyone who currently uses Python 
(that is popular on a Raspberry Pi already), may enjoy switching to 
Nim and gaining some additional benefits, without much of a switch over cost time 
wise.


### How to Install Nim on a Raspberry Pi?

Want to try Nim out on a Raspberry Pi? 

Luckily there are lots of ways to achieve this, so below are some of the 
easier methods I know of, including my preferred 
approach of installing from the Nim source code, and building it my self.

All the options shown below assume you have a Raspberry Pi already installed 
and working with Raspbian 10.x *Buster* that can be 
[download from here](https://www.raspberrypi.org/downloads/raspbian/). Personally 
my Raspberry Pi all run headless, so they use the *Raspbian Buster Lite* image, but 
if you use the desktop version, that should not make any difference to the 
information outlined below.

Make sure you have a working Raspberry Pi and have access to the command line, 
before following these steps. Then choose one of the methods outlined - where the 
third option is my own preference, but does take the longest to complete due to the 
build process. Not *that* long though of course!

<hr>

**1. Install from Raspbian Packages**

The most obvious (and probably the easiest) approach is to install using the 
package manager that is provided with *Raspbian* called `apt`. This can be run from 
a *Terminal* command line window. To ensure you have the latest package 
information, first run these two commands, which download the available 
packages list, and then updates your system with any new software versions, which 
may include security updates:
```bash
sudo apt update
sudo apt -y upgrade
```

To look at the information and version of the *Nim* package available use 
the command:
```bash
apt info nim
```

Currently (16 May 2020) the version of Nim available is very old at 
version: **0.19.4-1**. The current stable version of Nim is: **1.2.0**. For that 
reason I would suggest **not** installing using this approach, and instead use one 
of the other options below instead. That way you will get a far more up to 
date Nim experience!

However, if you do want this older version for some reason, the command to 
install it would be:

```bash
sudo apt get install nim nim-doc
```

Hopefully, the Nim package maintainer will bring the available Raspbian package 
back up to date soon, and this method of installation will be more suitable.

<hr>

**2. Install with the 'choosenim' tool**

[choosenim](https://github.com/dom96/choosenim) is a tool written by Dominik Picheta who 
is the author of the great [Nim in Action](https://www.manning.com/books/nim-in-action) 
book. As it states on the publishers web site:

> Dominik Picheta is one of the principal developers of Nim and author of the Nimble package manager.  

What `choosenim` can do is not only install the `nim` compiler onto a Raspberry 
Pi, but also then manage the process of keeping it up to date, manage the use 
of different Nim compiler versions, and more.

Before installing and using `choosenim` it is worth making sure the Nim 
dependencies are installed in Raspbian first. This can be 
done with the following commands.

In a Terminal command line window, first update the packages, and then ensure
your Raspberry Pi has the latest packages and security fixes installed too:
```bash
sudo apt update
sudo apt -y upgrade
```

Now install the the `build-essential` meta package which will install the *GCC* compiler, 
and other basic C language tools needed by Nim. The `openssl` library may already be 
installed, but it does no harm to double check, and the `curl` program is needed in the next 
step to download and run the `choosnim` installation:
```bash
sudo apt -y install build-essential openssl curl
```

You can now follow the instructions provided on the 
[choosenim](https://github.com/dom96/choosenim) site. For Unix (such as Raspbian) 
the following command should commence the installation:
```
curl https://nim-lang.org/choosenim/init.sh -sSf | sh
```

<hr>

**3. Install by compiling Nim from source**

The instructions below are my preferred method of installing Nim on a Raspberry Pi. 
Although it takes a bit longer, as it does a complete build of the compiler and 
supporting tools, it is a good way to have an insight into how the Nim compiler 
is created.

Before building from source, the Nim dependencies need to be installed in 
Raspbian first. This can be done with the following commands.

In a Terminal command line window, first update the packages list, and ensure
your Raspberry Pi has the latest packages and security fixes installed too:
```bash
sudo apt update
sudo apt -y upgrade
```

Now install the the `build-essential` meta package which will install the *GCC* compiler, 
and other basic C language tools needed by Nim. The `openssl` library may already be 
installed, but it does no harm to double check, and the `curl` program is needed in the next 
step to download the source code from the Nim web site:
```bash
sudo apt -y install build-essential openssl curl
```

The Nim source code is already bundled up in a format ready to be built, and can be 
obtained from the [Nim Unix download page](https://nim-lang.org/install_unix.html).

You should check the '*Source archive*' link on the above page, and if needed 
change to the latest stable version available. Currently the linked file name is for 
source code version: **nim-1.2.0.tar.xz**.

The steps below create a new directory called `scratch` in you home directory,
change into that directory, and then it uses `curl` to downloaded the source code 
file to your Raspberry Pi. The `tar` command then extracts the archives contents, and 
the final command changes into that new directory containing the Nim source code:
```bash
cd ~
mkdir scratch
cd scratch
curl https://nim-lang.org/download/nim-1.2.0.tar.xz -O
tar -xJvf nim-1.2.0.tar.xz
cd nim-1.2.0
```

Now the actual build process can begin! First run the command to commence the build:
```bash
./build.sh
```

Once that completes, the build of the `koch` program can be done with an existing 
older supplied version of the Nim compiler with the command:
```bash
bin/nim c koch
```

The newly created `koch` tool can now be used to compile so supporting tools:
```bash
./koch tools
``` 

Next, run the command with `koch` to create the `nimble` package manager tool:
```bash
./koch nimble
```

Now, using the Nim provided `install.sh` script, install everything 
into: `$HOME/.nimble` directory. The second step below also copies the 
Nim tools into the same location as the Nim compiler as well:
```bash
sh ./install.sh $HOME/.nimble
cp ./bin/* $HOME/.nimble/nim/bin/
```

Next it is worth updating the `nimble` package managers package manifest, 
and then using `nimble` to update itself to the latest version. The last command 
moves the original (older) version of the `nimble` program to be called `nimble-orig`. 
That way the newer version is run instead of the older one, that was built 
from the source code downloaded. Run these three steps as:
```bash
nimble refresh -y
nimble install -y nimble
mv $HOME/.nimble/nim/bin/nimble $HOME/.nimble/nim/bin/nimble-orig
```

To ensure the `nim` compiler and supporting tools, plus `nimble` that were all 
built on the Raspberry Pi computer are available in your `PATH`, run the command:
```bash
export PATH="${PATH}":$HOME/.nimble/nim/bin:$HOME/.nimble/bin
```

The above line should also be added to your you `~.profile` or `~.bashrc` file 
(or equivalent for other shells) so that the `PATH` is updated each time you 
login.

That's it!

You should now have a nice pristine new stable version of Nim tools, the `nim` 
compiler, and the `nimble` package manager. To check they are working, their 
version numbers can be displayed with the following commands:
```bash
nim --version
nimble --version
```

If you would prefer to perform all the above steps in a single command, I run 
them using a `bash` script called: `nim-install.sh`. This can be found in my 
[GitHub repo](https://github.com/wiremoons/GenIsys-Pi4/tree/master/install).

If you wanted to use it, it can be download, and then run using the commands:
```bash
mkdir ~/scratch
cd ~/scratch
curl https://raw.githubusercontent.com/wiremoons/GenIsys-Pi4/master/install/nim-install.sh -O
chmod 755 ./nim-install.sh
./nim-install.sh
```
Once run, you will still needed to change your `PATH` in your you `~.profile` or `~.bashrc` 
files as outlined above. 

If you want some more setup examples, my above 
[GitHub repo called 'GenIsys-Pi4'](https://github.com/wiremoons/GenIsys-Pi4/) 
contains all the files I clone onto a Raspberry Pi when is set it up from a 
fresh Raspbian install.

The [Nim web site Unix install page](https://nim-lang.org/install_unix.html) also 
has plenty of helpful information, and offers more alternative install options such as 
using `docker` or `snaps` too. 

### Update

It was pointed out to me on the [Nim forum](https://forum.nim-lang.org) that 
there are also pre-complied Nim binaries for many different platforms, including 
the Raspberry Pi, available from the 
[Nim Github release pages](https://github.com/nim-lang/nightlies/releases). 
Just in case you don't want to use one of the above  *build it yourself* options!


### What Next?

Now you have the latest stable version of Nim installed, you can start to develop 
your own programs. There are some very good tutorials on the 
[Nim 'Learn'](https://nim-lang.org/learn.html) section of the Nim web site.

A fairly simple Nim program I wrote is used to check the CPU temperature of my Raspberry 
Pi. For more information see the page here: [systemp](https://github.com/wiremoons/systemp)

If you want to find out more about Nim, visit the great web site pages here: [Nim language site](https://nim-lang.org/) or if you need more help, the [Nim Forum](https://forum.nim-lang.org) is a friendly and supportive community location to call on.


### Article Details

- Title: *{{ page.title }}*
- Published Date : {{ page.date | date: "%d %b %Y" }}
