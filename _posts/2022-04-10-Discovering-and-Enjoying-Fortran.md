---
title: "Discovering and Enjoying Fortran"
description: "Some personal views on discovering, using, and enjoying modern Fortran." 
published: true
date: 2022-04-03
tags : [ "fortran", "language", "fpm", "development", "source_code" ]
categories : ["development", "fortran", "source_code" ]
---

# Discovering and Enjoying Fortran

## Background

I have for approximately a year been using [Fortran](https://fortran-lang.org).
It is a very interesting computer programming language. It has a very long
history, is pleasant to program in, has a fantastic, welcoming, friendly and
[knowledgeable community](https://fortran-lang.discourse.group).

As a bit of context, I am not a professional developer, but I have worked
in ICT areas all my career, doing many different roles. Despite this, for
some crazy reason I also like to spend some of my spare time programming.
This is mainly due to the creativity and mental challenge it supports and
provides, which I really enjoy (yes - my wife thinks I am weird too). Apart
from attending school, and a bit of college, I do not have any great academic
qualifications, and I have not worked much with the academic establishments
either, so I am not too familiar that specific culture.

I have over many years tried lots of different programming languages — but only
recently found Fortran. The _blame_ for this is Milan Curcic's excellent book
[Modern Fortran](https://www.manning.com/books/modern-fortran) which caught my
curiosity by accident one day. Ever since reading it, I am intrigued, and keep
exploring more of the language and its eco system.

Previously, I mainly enjoyed programming in the [Ada](https://learn.adacore.com/courses/intro-to-ada/index.html) 
language, but until very recently the Ada GNAT compiler was not supported on _Apple Silicon (M1)_ computers 
natively, so this created an _opportunity_ to look at other
alternative languages such as modern Fortran. Unfortunately, Ada support for
the latest version of *AdaCore's Community Edition* was also dropped for Apple computers, 
which effectively alienates in many ways the use of Ada as a sensible choice for
macOS. So, I was keen to find a language to adopt as a replacement.

When looking into modern Fortran, it became quite clear from all the
introductory explanations that is was aimed at scientists, and that its
targeted developer is not really general purpose computing, or systems
programming. That's fine, but again I was intrigued as to why a language could
have such a specific and specialised audience... time to explore further!

I am certainly not a scientist of any kind, so was a bit concerned that my
complete lack of '_fitting into the advertised point of Fortran_' would soon be
an issue:

> Fortran is mostly used in domains that adopted computation early--science and
> engineering. These include numerical weather and ocean prediction,
> computational fluid dynamics, applied math, statistics, and finance. Fortran
> is the dominant language of High Performance Computing and is used to
> benchmark the fastest supercomputers in the world. If you're writing a program
> or a library to perform fast arithmetic computation over large numeric arrays,
> Fortran is the optimal tool for the job.

Oh dear! That is a lot if instant credibility on display. I am certainly not
going to be pretending to be able to use Fortran for its stated purpose, so I
guess this article and any criticism of Fortran are all my own fault for
choosing something that really is not meant for me in the first place...
perhaps.

I am (naively?) hoping I have not made a silly mistake, but instead found a
hidden gem of a language! Well hidden in plain sight for more than half a
century. After all, if it is still being utilised today after approximately 65
years in use, it must have some thing to offer me too.

Curiosity got the better of me, and I told myself I might even learn something
from the professionals using it to model the weather, fluid dynamics, tidal
changes, medical research, and many other amazing and often unnoticed
achievements mankind is involved with day to day, that we all benefit from.

Having the great opportunity to hang out with some of the scientists of the
world for some of my spare time was appealing, if not daunting. I just hoped I
didn't get noticed and called out as a _charlatan_ by accidental association:

> a person falsely claiming to have a special knowledge or skill.

My apprehension and concerns were supported, as there are lots of interesting
(but perhaps not always comprehendible to me!) conversations in the community on
scientific areas — which is an insight, and goes to prove I definitely did not
miss my calling in life. Although it is forcing me to learn a bit more
mathematical cognition (which can't be a bad thing for anyone), it is not a
pre-requisite for participation.

So to be clear, despite the target language use, it is absolutely by no means
elitist, exclusive, closed, or judgemental in any way at all. The community is
extremely welcoming, open, helpful, supportive, and encouraging. So, while not
being a scientist of any kind, that is certainly no reason not to enjoy Fortran
or the company of the kind intelligent people you will get to interact with, if
you wish.

So, with that context, what do I think of Fortran for a general development
language? What would I change? How can I help perhaps?

## Observations on Fortran

The purpose of these views is to share my own observations of how I have
(perhaps wrongly) understood these Fortran language areas to work. I am
obviously using Fortran from a generalist perspective, and many of these issues
might be just historical, intentional, or I have not found the right approach
yet.

First of all — why use modern Fortran in the first place?

- The language is _easy to learn_, especially if you have any prior programming
  experience at all;
- The language is _wordy_ but therefore (to an English speaker at least) should
  be understandable, if not fully comprehendible;
- The layout of the code is logical and uses common standard programming
  language terms (ie procedure/function/integer/assignment/etc);
- There are no complex header files and source code files - just single
  '`module`' files of code that are _used_;
- There is a standard library, package manager, good documentation, GitHub code
  examples, books, videos, training courses, and more;
- There is a choice of different Fortran compilers — both commercial and
  opensource;
- The language is standardised and therefore clearly defined as a baseline;
- The language is supported well on all major (and many legacy!) platforms such
  as Windows, Linux, and macOS;
- The language is compiled and has a reputation for being very fast to execute.
- It has active community who are keen to modernise and keep Fortran going, and
  expand and grow its usage as much as possible too.

Sounds perfect, what are the concerns or areas for improvement?

- It has a lot of history resulting in quite a bit of '_older_' code less
  understandable legacy code (ie FORTRAN vs Fortran);
- It is a domain specific language for good reasons — the modernisation work
  will help open this area up though;
- It is too dependant on other languages and uses them as a '_crutch_' and for
  '_work arounds_' to fill in areas of poorer capability;
- It has lots of duplication of effort historically trying to fix limitations,
  often done unintentionally in isolation, but for the right reasons;
- The standards that it benefits from may also be hindering (speed wise) its
  modernisation;
- It needs to keep broadening its usage and grow outside its current solid
  bastion of use - as an enhancement and expansion - not as a replacement in any way!
- It may have legacy code and a history — but it should be proud of that;
- it is not a real hindrance for '_Fortran_' to be so mature, and it should not
  be presented itself as 'old' anymore - modernisation is underway after all.

What words would I use to describe modern Fortran?

> Mature, friendly, tried and tested, supportive, wise, reliable, dependable,
> fast, clever, well thought out, clear, unique, modern, understandable.

## What are some specifics in more detail?

The following are the areas that bother me the most about Fortran, perhaps just
because I don't _get it_, or have missed something along the way perhaps.

### Pre-processor use:

By default, a lot of the `stdlib` source
[see 'src' sub directory](https://github.com/fortran-lang/stdlib/tree/master/src)
is stored in the GitHub repo as quite a lot of `.fypp` processor files. This
made me wonder:

- Initially I was very confused by them as a newcomer to Fortran and wondered
  what they even were, when I was expected just to see `.f09` files.
- Once I figured out they were for performing pre-processing, and that another
  programming language was needed (ie Python), I was a little worried that the
  language I was interested in developing in, actually needed a whole other
  language to be able even use it. I didn't really want to have to
  use/install/learn/trouble shoot Python just to use Fortran... but I got over
  it I suppose. I do draw the line at having to install an whole different eco
  system such as [Anaconda](https://www.anaconda.com/). Not because I have any
  issue with it (I have used it in the past for other reasons) — just that I
  want to learn and use Fortran, not something else, before I even get going.
- One reason for being interested in Fortran was because it has a modern package
  manager called [fpm](https://fpm.fortran-lang.org/en/index.html). Having used
  similar for other languages package managers (Ada - `alire` / OCaml - `opam` /
  Rust - `cargo` / .Net - `nuget` / etc), I was please to find a the adoption of
  a modern concept!

### Limited Support for connectivity:

One of the first areas I noticed (which of course should not be a surprise given its history) is the lack of easily 
accessible native support for network connectivity. So, currently using native Fortran it is not 
possible to request a web page, or Rest API to obtain some JSON data. Fortran can utilise C language code 
well, so it can get around this by calling other applications and libraries such as [curl](https://curl.se) to 
perform such tasks. There is work planned to include better support via the Fortran stdlib, but is work in progress 
currently. For now (like a few other areas), Fortran relies on support from other languages to 
achieve connections beyond its native language realm.

## Highlights of Using Fortran

There have been quite a few notable highlights for me since I started my Fortran
adventure, which I thought it would be important to include, as it creates some
balance with the negative points made.

**Monthly Fortran YouTube Videos**: this is a great resource and window into the
world of Fortran! Each month, a team of regulars take timeout of the busy lives
and meet up virtually for a video call, to discuss key topics relevant to the
Fortran community. This resource is a lovely opportunity to hear first hand from
many of the key stakeholder, professionals, and core contributor to the many and
varied Fortran projects and initiatives. This is not only handy to understand the
context behind some of the Discourse discussions, but also put faces to names,
and to gain a more in depth understanding of the issues and challenges being
faced to ensure the modern Fortran initiative maintains momentum, commitment,
and a collaborative focus. Enjoy the recorded monthly videos on YouTube here:
[Fortran Programming Language](https://www.youtube.com/channel/UCTYRAlVmMCGGcrMkKxQLurw)
or look out for the planned agenda and live call details on the
[fortran-lang Community Discourse](https://fortran-lang.discourse.group).

**Advent of Code in Fortran**: I have been slowly working my way through the [Advent of Code](https://adventofcode.com) 
annual challenges, often using them to try out different languages, and as a way to 
structure my learning of programming in a fun and purposeful way. Last year (2021) I decided to 
give Fortran a go, as the language to use to solve the puzzles. It was very enjoyable, and a few of 
the community also participate as well each year. I certainly learnt a lot about Fortran! It enabled me to 
consider other approaches to programming, due to the way the Fortran language 
uses arrays much more, and has very strong features for their manipulation. I did of course 
get stuck a lot, and have yet to complete every challenge - but that is down to my 
limitations, not the language!

## What's Next?  

I am certainly going to continue to involve myself with the Fortran language, and the 
welcoming and helpful community. While I am still exploring other languages too - I am glad 
I stumbled across Fortran, and look forward to continuing my journey to learn more.

There are exciting times ahead for the language, such as:

- the rapidly evolving [LFortran](https://lfortran.org) brand new opensource compiler! 
- the [Fortran Standard Library (stdlib)](https://stdlib.fortran-lang.org/) with its growing list of modern language capabilities;
- the fantastic [Fortran web site](https://fortran-lang.org/en/) full of documentation, tutorials, and other background information;
- the constantly improving [Fortran Package Manager (FPM)](https://fpm.fortran-lang.org/en/index.html) and 
its many [Packages](https://fortran-lang.org/en/packages/)

If you are curious, why not pop in to the [Fortran Discourse](https://fortran-lang.discourse.group) and 
see what people are up to, or just hang out there to learn more... it is certainly a 
exciting time to be involved in the evolution of a great programming language!


### Article Details

- Title: _{{ page.title }}_
- Published Date : {{ page.date | date: "%d %b %Y" }}
