---
title: "Ada Language Introduction"
description: "Brief introduction into the Ada language"
published: true
date: 2020-12-19
tags : [ "ada", "language", "ada-lang", "introduction", "compile" ]
categories : ["development", "ada", "introduction" ]
---
# Ada Language Introduction

## Background

While exploring different computer programming languages on the *Rosetta Code* web site, I came across a language called [Ada](http://rosettacode.org/wiki/Category:Ada). The syntax looked interesting, and the code while a bit more verbose than I was used to, was understandable, and looked logical and well formed. I was intrigued!

So started a new journey of learning and exploration &mdash; primarily out of curiosity to start with. I found quite a few online resources and started to read. Consequently, for most of 2020 I have spent quite a bit of time learning more about the Ada language, and so far it has been an enjoyable journey!

Ada is a well used language, although due to the nature and purpose of many of its programs, there is a not a massive amount of 'open source' code, at least to the same extent as *C*, *Python*, *Java*, or *Rust*. Nonetheless, if you look, it can be found, and what is about is normally of very good quality, and written by people who are just as passionate and helpful.

Below is a brief introduction for the Ada language. There are plenty of other overviews about, but these are the key points I noted.


## About the Ada Language

Ada is a general purpose compiled systems programming language. Ada is heavily used in embedded real-time systems, many of which are safety critical. While Ada is and can be used as a general-purpose language, it will also really shine in low-level applications such as:

* Embedded systems with low memory requirements;
* Situations where no garbage collector is allowed;
* Direct interfacing with hardware;
* Soft or hard real time systems;
* Low level systems programming.

Specific domains seeing Ada usage include aerospace, defence, civil aviation,  rail, and many others. These applications require a high degree of safety: where a software defect is not just an annoyance, but may have severe consequences.

Ada code has facilitated such massive software projects such as the *Space Station* and the *Paris Metro*. These are just two examples of projects where safety is critical.

The first Ada standard was issued in 1983; it was subsequently revised and enhanced in 1995, 2005 and 2012, with each revision bringing useful new features. The current Ada languages standard is *Ada2012*, although a new language standard is expected soon which is anticipated to be *Ada2020* (or due to COVID-19 sometime in 2021 now perhaps...). 

Prior to the first released version as Ada1983, there were a number of proposed versions of Ada used for the selection of the final Ada language standard. These initial proposed versions had the code names:

* **Strawman** - a version to test the proposed plans for the language by the internal working group;
* **Steelman** - version in 1978 that firmed up on the language design and requirements;
* **Colour versions** - four competitors were selected (Blue, Green, Red, Yellow) for the first phase of a design competition;
* **Ada** - the name for the language was chosen in 1980, named after *Ada Countess of Lovelace*.

The Ada language became *ISO standard 8652* in 1987. The maintenance of the language is performed by the *Ada Rapporteur Group (ARG)* of *ISO/IEC Committee SC22/WG9*. The ISO standard continues to evolve and to be up dated inline with Ada language releases, with the last ISO8652 revision being provided shortly after Ada2012 was released.

The official released versions of Ada are:

* **Ada 1983** : the first version also called '*green*' primarily used by US DoD;
* **Ada 1995** : added new features supporting systems, financial applications and for objects (OOP);
* **Ada 2005** : additional language features such as interfaces;
* **Ada 2012** : inclusion of dynamic contracts and tasking;
* **Ada 2020** : was anticipated for release in 2020, so final name to be confirmed.

Other related areas or dialects are *Ravenscar Profile* and *SPARK*. The Ravenscar profile was added into Ada 2005, and enables the development of real-time programs with predictable behaviour. SPARK is a well defined subset of the Ada language that uses contracts to describe the specification of components to add support for both static and dynamic verification. Both these are separate topics in their own right.

The language is an international standard. The open source *GCC* compiler suite includes support for Ada as `gnat`.

This open source Ada compiler is also offered commercially as **GNATPro** from a company called **AdaCore**. AdaCore also contributes significant development from its commercial versions back into the open source GCC GNAT version, as well as offering many '*Community*' versions of their tools as well.


## Key Features of the Ada Language

Ada is a *multi-paradigm* language supporting object orientation and some  elements of functional programming. Its main core focus however is imperative (or procedural) much the same as the C language.

The source code of Ada is **case-insensitive**, unlike most other languages. Ada is a **statically typed** language, so assuring code safety. It is also **nominative**and **strongly** typed. Ada supports **tasks** naively that are used to support multiple CPU computer processors. It has a full runtime library that was seen as large at one time, but compared to modern languages is now quite small.

Ada includes both static checking performed by the Ada compiler, and dynamic checking — carried out through code assertions.

Some of the stated downsides of Ada language use are that it is sometimes perceived as complex &mdash; when compared with the C language. Ada has a large standards based specifications, making it complex to understand fully. There are a lots of types &mdash; for example there are more than ten types for *String* alone. It is *designed by committee* which is often seen negatively - but could quite easily be seen as a benefit too in some development scenarios... Many of these negatives are now outdated, but often persist as views perpetuated on the internet still. 

To me Ada is robust, flexible, modern, clearly defined, builds fast statically compiled programs, and is fun to work with. As a hobby programmer I get to pick from any and all available languages, and Ada is certainly my favourite so far!


## How to Get More Information

More information and support for installing Ada can be found at the site:

- [Get Ada Now](http://www.getadanow.com)

The AdaCore tools and libraries are all on GitHub here:

- [AdaCore GitHub Site](https://github.com/AdaCore)
    
There are plenty of good articles for more background on Ada such as:

- [Wikipedia Ada Language page](https://en.wikipedia.org/wiki/Ada_(programming_language))    
- [AdaCore - resources page](https://www.adacore.com/resources)

One great free resource to learn the Ada language so you can write your own programs is available here:

- [Learn Ada Course](https://learn.adacore.com/courses/intro-to-ada/index.html)

You can also download a nice PDF book to read of the above course here:

- [Learn Ada Course (PDF)](https://learn.adacore.com/pdf_books/courses/intro-to-ada.pdf)



### Article Details

- Title: *{{ page.title }}*
- Published Date : {{ page.date | date: "%d %b %Y" }}
