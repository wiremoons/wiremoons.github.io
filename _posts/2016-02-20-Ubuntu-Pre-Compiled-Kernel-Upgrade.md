---
title: "Ubuntu Pre Compiled Kernel Upgrade"
description: "Quick overview on how to upgrade Ubuntu to use a newer kernel - without compiling a new Linux kernel yourself"
published: true
date: 2016-02-20
tags : [ "linux", "ubuntu", "kernel", "surface pro 3" ]
categories : ["linux", "ubuntu", "upgrade kernel",]
---
# Ubuntu Pre-Compiled Kernel Installation

## About

If you are not familar with Ubuntu - a good overveiw of this execellent free
operating system's features can be found here: [Ubuntu](http://www.ubuntu.com/desktop/features)

The instructions below are for obtaining and installing a pre-compiled Ubuntu
kernel, from the [ubuntu.com kernel web site](http://kernel.ubuntu.com/).

The instructions explain how a computer already running the Ubuntu operating
system can use the latest Ubuntu kernel available from the Ubuntu Kernel web
site.

The kernels are provided by the Ubuntu community, and are pre-compiled for you
- so all you have to do is download and install the one you want.

This approach will often let you enjoy the latest Linux kernel features and
obtain any performance improvements included - without having to wait for your
Ubuntu distribution release to be updated first, or go through the compile
process yourself!

These kernels include the Ubuntu specific features and patches, but are built
using the mainstream Linux Kernel source code also - so the best of both
worlds if you are a Ubuntu user.

Obviously bear in mind that these kernels are installed at your own risk - so
if you have doubts about doing this, it is probably best to wait until the
newer kernel is included as part of a software update, or newer Ubuntu
release.

I choose this route as I needed a number of newer kernel fixes to ensure my
Microsoft Surface Pro 3 Type Cover keyboard worked properly, and as a
consequence I also obtained newer kernel features and improvements too! I did
not fancy going through the compile process myself - as it takes a little while
to compile a Linux Kernel on the Surface Pro 3 computer, and this method is
practically instant. 

As a sideline - the [Reddit Surface
Linix](https://www.reddit.com/r/surfacelinux) pages are a great source of
support and information for running Linux on this particular hardware.


## Obtaining the Ubuntu Kernel

The Ubuntu kernel needs to be downloaded from the Ubuntu web site located
[here](http://kernel.ubuntu.com/~kernel-ppa/mainline/):

	» http://kernel.ubuntu.com/~kernel-ppa/mainline/

Scroll down the above web page to locate the kernels available for your
current distribution (ie Ubuntu 15.10 is called 'Wily'), and then select the
highest kernel version you want to use. Note that some of the kernels provided
are 'release candidates' - identified by 'rc' in their names. Choose a non
'rc' version if you need stability over the very newest features and of course
potential bugs.

For example the latest Wily (Ubuntu 15.10) kernel version 4.4 can be found in
the directory [here](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/) at the time of writing:

	» http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/

Additional higher versions will be added from time to time, so check for
additional v4.4.x kernel versions as needed.

Once in the correct directory for the kernel version that you have chosen to
use, you will see a selection of files. If you want to know what improvements
and fixes the particular kernel version includes, then look in the file
called: 'CHANGES'. There is also a file called 'README' in each directory too
- that contains additional helpful information.

If you wish to proceed with trying this particular kernel on your own
computer, you need to download the following three '.deb' files for each
kernel installation - assuming you don't need any of the alternative
specialised kernels of course!

The computer used in the example described below is a Microsoft Surface Pro 3,
which is a 64bit computer and is running Ubuntu Wily 15.10 64 bit operating
system. Your own needs may differ - if the generic kernel is unsuitable to your
own use case or computer hardware, then select the corresponding alternative
files to those presented here - the overall process is the same though.

Note that in the file names shown below, the 'xxxx' element is the version
number of the kernel - so it will vary based on your own kernel version number
selection.

The files to download are:

The Linux headers specific to you machine type (ie amd64) and kernel
usage (ie generic) - so file 'linux-header-xxxx-generic-amd64.deb':

	» linux-headers-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb

The 'linux-headers-xxxx-all.deb' which is generic to any of the other
two files listed here (ie the other headers and image files):

	» linux-headers-4.4.0-040400_4.4.0-040400.201601101930_all.deb

The Linux kernel image file is the third file needed, so to match the above
files already downloaded, obtain 'linux-image-xxxx-amd64.deb' - again specific
to your machine (ie amd64):

	» linux-image-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb

The above three files are needed for each Linux Kernel upgrade on a 64bit
generic computer.

Download them and put them in their own directory on your computer, so they
don't get mixed up with other similar kernel versions and files you may have
already downloaded.


## Install the New Kernel

Once the three files are obtained, you can install the newly downloaded kernel
`.deb` files (ie see instructions above). The three files obtained that are
required for each upgrade are:

	» linux-headers-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb
	» linux-image-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb
	» linux-headers-4.4.0-040400_4.4.0-040400.201601101930_all.deb

These can be installed with the command line below - where the three files are
the only ones on the current directory of course:

	» sudo dpkg -i linux-headers* linux-image*

If after installing the kernel using the above command, there are dependency
issues reported, you can run the following command to try to fix them:

	» sudo apt-get -f upgrade

You will also need to update update Grub to ensure the computer boots
from the the new kernel with the command:

	» sudo grub-install

You should now be able to reboot and test your new kernel installation!

Once you reboot your computer you can check what kernel version it is running
with the command:

	» uname -a
	
## Fixing Issues with Boot Problems

Hopefully the above process runs well, and your computer runs the newly
installed kernel without issue. 

Before you install however, you should really familiarise yourself with howto
fixing Ubuntu boot problems - just in case! This information is worth knowing
anyway...

The Techradar web site has a good article which maybe of interest [Techradar -
How to repair Ubuntu if it won't
boot](http://www.techradar.com/how-to/computing/how-to-boot-repair-ubuntu-1315203)

There are of course plenty of other helpful resources on the internet too
should you run into problems - just search on
[Google](https://www.google.co.uk/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=ubuntu%20boot%20failure)
for solutions!

## Updated Information - Wily Kernel 4.4.3

On the 25 feb 2016 a new version of the Wily-4.4.x Ubuntu kernel was made
available as
[v4.4.3-wily](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.3-wily/).

Based on the artcile above (ie to suit a 64bit generic kernel requirement) I downloaded the
following files:

 - [linux-headers-4.4.3 generic](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.3-wily/linux-headers-4.4.3-040403-generic_4.4.3-040403.201602251634_amd64.deb)
 - [linux-headers-4.4.3 all](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.3-wily/linux-headers-4.4.3-040403_4.4.3-040403.201602251634_all.deb)
 - [linux-image-4.4.3 generic](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.3-wily/linux-image-4.4.3-040403-generic_4.4.3-040403.201602251634_amd64.deb)

Save the above three files in a directory on there own, and then run the
following command in the Ubuntu Terminal, in that directory:
```
sudo dpkg -i linux-headers* linux-image*
```
Once the new Ubuntu Kernel files where installed, I then ran the following two
commands (just as a precation) - but no additional packages were installed on my
particular system - that may vary for yours of course!:
```
sudo apt-get -f upgrade
sudo grub-install
```
Follow the other steps in the main article above if you need more information.

The are a significant number of patches and files in this v4.4.3 kernel - as
listed in the accompanying
[CHANGES](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4.3-wily/CHANGES)
file.

