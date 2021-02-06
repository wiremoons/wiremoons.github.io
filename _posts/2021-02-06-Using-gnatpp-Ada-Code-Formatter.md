---
title: "Using gnatpp Ada Source Code Formatter"
description: "Formatting Ada source code files with Pretty Print tool called gnatpp"
published: true
date: 2021-02-06
tags : [ "ada", "language", "ada-lang", "format", "gnatpp", "gnat", "source code" ]
categories : ["development", "ada", "source code" ]
---
# Using gnatpp Ada Source Code Formatter

## Using gnatpp

The GNAT pretty print tool can format Ada source code files, to ensure they 
follow a set of formatting rules. The program is named: `gnatpp`.

On Ubuntu 20.04-02 LTS it is included in the package named: '*asis-programs*' 
so can be install with the command:

```bash
sudo apt install asis-programs
```

If you are using the AdaCore GNAT Community Edition 2020 than it comes 
pre-installed with that distribution. To obtain more information about 
using '`gnatpp`' on the command line you can run the program and view the 
help output:

```bash
gnatpp --help
```

An example command used directly from the CLI is shown below, which will 
reformat all the Ada body and spec source files in the current directory:

```bash
gnatpp -i3 -v -w -rnb *.adb *.ads
```

The above will format all Ada source code files (ie `*.adb *.ads`) with 
three (3) spaces indent, and display output verbosely and include any 
warnings. 

**WARNING**: The command **will also replace the existing source files with 
the newly formatted versions, without creating backups of the originals**. If 
you want to try the program out first, either make sure you have backup of 
the files first, or remove the '`-rnb`' option from the command above, which 
stands for '*replace no backups*'. 

An alternative approach, and probably easier one, is to add a 
'*Pretty_Printer*' section to the projects '*.gpr*' file, and include the 
preferred settings to be used by `gnatpp` when executed.  An example 
configuration extract from a project file is below:

```ada
-- to apply run :  gnatpp -Pmyproject.gpr
package Pretty_Printer is
   for Default_Switches ("ada") use ("-i3", "-M120", "-v", "-w", "-rnb", "-A1", "-A2", 
      "-A3", "-A4", "-A5", "--no-separate-is", "--no-separate-loop-then");
end Pretty_Printer;
```

To apply the above `gnatpp` settings to all the source code files included in 
the project, the following command can be used:

```shell
gnatpp -Pmyproject.gpr
```

On occasion a source code file may contain a block of code that should not be 
formatted by `gnatpp`  when it is run. In order to temporarily disable the 
processing within a section of the source code, the areas to be left 'as is' 
can be marked with '`--!pp off`' to disable processing, and then '`--!pp on`' 
to re-enable the process when ready to resume the normal formatting rules. 
An example to stop the reformatting of an array block is shown below:

```ada
-- more code to be formatted above

subtype Word is String (1 .. 3);
type Word_List is array (Positive range <>) of Word;

--!pp off
-- above disables gnatpp formatting of array block below
Words_List_Array : constant Word_List :=
   ("aah", "aal", "aas", "aba", "abb", "abo", "abs", "aby", "ace", "ach",
    "act", "add", "ado", "ads", "adz", "aff", "aft", "aga", "age", "ago",
    "zho", "zig", "zin", "zip", "zit", "ziz", "zoa", "zol", "zoo", "zos",
    "zuz", "zzz");
--!pp on
-- above re-enables 'gnatpp' formatting

-- code to be formatted continues below
```

There are lots of options to suit most peoples preferences for formatting 
their Ada source code, so be sure to read the commands help output. The source 
code for the `gnatpp` command is available on GitHub in the 
[AdaCore libalang-tools](https://github.com/AdaCore/libadalang-tools) repo – 
look in the '*src*' sub directory for the source codes files starting 
with '*pp-*' in their names.

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
