---
title : "Serving Up Static Content from Golang Apps"
description : "Some ramblings about Go applications that require content in their binary"
published: true
layout: post
date : 2014-11-30
tags : [ "go", "golang", "webserver", "assets", "static content", "binary packaging" ]
categories : ["development", "golang", "go" ]
---
## The Background

[Go][1] is a great programming language, and three of the features I like the most are:

1. **its easy cross platform program creation** - write a program on a Linux 64bit system&mdash;cross compile easily, and deploy the executable straight onto a Windows 32bit computer (if you so desire of course!);
2. **its modern awareness of the web** - and therefore inclusion of commonly required application functionality built-in to the language libraries;
3. **its static compilation** - related to the first one, but key in that it is only the executable that needs to be deployed&mdash;no library dependency, DLL hell, or installation packaging needed. 

What I dont like to much at the moment&mdash;or probably more correctly&mdash;I am coming to terms with, is the different approach is the lack of a standard graphical user interface (GUI) libraries. There is lots of work going on to port the usual suspects ([GTK][2], [QT][3], [ncurses][4], [QML][5]), and there are some good native Go libraries about too ([termbox-go][6]) and of course the ability to use a web interface.

So while I am personally quite happy with the command line interface (CLI), and most quick applications I write are just that, if you ever want to make your computer program accessible to the majority of computer users, a familiar GUI is required.  

So, I thought I would have a go at creating an application that is managed and controlled by the user through a web browser. My inspiration for this is the fantastic open source application called [Syncthing][19]. When you start this application (you can optional disable this behaviour of course!) it will open the users web browser, and provide a very modern (using [Bootstrap][8] and other code) interface, that is intuitive, informative, instructional, and independent. 

What do I mean by this?  Well:

 - **intuitive**: when you see the interface you immediately get an idea of what it is for, and how you might interact with it&mdash;it's in the web browser which you use frequently and are familiar with (hopefully!);
 - **informative**: the interface contains useful functional information that serves a purpose, and therefore inspires confidence that in will do what it is designed for;
 - **instructional**: the interface contains enough hints, labels, indicators, and meta help to ensure you are confident to interact with it&mdash;without fear it destroy you computer;
 - **independent**: the interface is self contained on one web page; doesn't sprawl onto other areas, doesn't needs other 'stuff' to make it work (expect the modern browser), and feels contained and manageable.

So that covers off the user needs, well a lot of the key *getting going* ones anyway!  

So how hard is it to make something as cool as [Syncthing][7]'s awesome offering? 

How does it work in the background?  How do I get a functional *under the hood* application to sync my files&mdash;and a cool user interface? Where does it store all the code, the web pages, the web-server, the Bootstrap, JavaScript, never-mind the actual programs original file-syncing purpose?

Well that's the journey I have begun, trying to figure out what I need to do to replicate this, or something close to it&mdash;and add a nice web browser based GUI to one of my programs, that is currently a CLI application only. 

Just to make it easy on myself the application I want to put a front end on is quite simple, but does use a database to store its information. This is currently in [SQLite][20]&mdash;so maybe I will write another post in the future about my quest on using non Go and native Go databases in my applications...

So, this article is going to just focus on managing the GUI interface content in the application, and specifically how it can be included in the application binary?


## The Problem to be Solved

So surely you just write you application, throw in a [simple Go web server][9], give it some HTML pages, chuck in some Bootstrap CSS and job done? 

Well that's what I first thought too, being an optimist when it comes to starting a new hobby programming endeavour! Just quickly wrap those add-ons around my code&mdash;job done&mdash;while the kettle is boiling ready for my next coffee break. I expect any professional programmer reading this is banging his head against the desk while cackling to themselves now!

So back to the five minute job. I thought I would start with a simple web server, and the Bootstrap integration, how hard can that be. I found on Google a great article by Greg Reinbach on building [Golang Web Apps][10] which fitted the bill perfectly. After being completely side tracked (see my previous article: ['Why Two Log Outputs?'][11]) I finally got a web server running. After also reading another helpful article  by @zeebo called ['Quick and Clean in Go'][13], I believed I was there!

I managed to create some static pages, and got a brief understanding of [html templates][12], chucked in the download Bootstrap CSS, tested it locally, and so was *good to go*&mdash;or not...

My application maybe used in an environment where it will not be able to access the internet&mdash;so it needs to be self sufficient. It also will run across different platforms (Linux, Mac and Windows), so the application should be a self contained binary to make upgrades easy, and therefore with as few external dependencies as possible (ie no database files; web pages; CSS style sheet; graphics; etc). 

No worries I thought, along with the the go application code I wrote&mdash;all my other dependencies files are laid out in directories alongside the Go code file (templates, static files, css, etc)&mdash;just as all the internet articles I read suggest (or more importantly I had assumed was implied)&mdash;so surely all that stuff I massaged together into a nice logical file structure gets included once my application is built right! After all Go builds self contained static binaries with all the required libraries and dependencies taken care of!

Wrong. Build the application and it runs fine locally. Excitably deploy it to another computer (or virtual machine), and you end up with panics, errors and screens of information telling you that you are obviously stupid and incompetent when it comes to programming, as the content cant be found any more!

Back to 'the Google' for some more advice and answers to question that are only provided in the context that you ask them in&mdash;so this time I asked with a wider context, less assumptions, and more perceived wisdom, the research restarted.

It turns out that clever Golang programmers embed all there files and external dependencies themselves within their application binaries. This is done with a mixture of voodoo that can include compression (ie zip, or gzip), base64 encoding, directory and file walking, pre-build work, and internal application clever path management of the then incorporated files. There are a few options around that can be used to achieved this:

1. Write your own - such as the Syncthing author @calmh did&mdash;reading (or in my case trying to unpick and decipher) the [Syncthing source code][7], it has build scripts and asset creation code which is very useful;
2. Use a native Golang tool for the job such as: [go-bindata][14], [go.rice][15] which provide an out of the box solution which I found via Google search;
3. Also courtesy of an [blog article][16] on this same subject, there is @mjibson's own [ecs][17], and another I didnt find [statik][18]. Thanks to the weekly [Go Newsletter][21] [Issue #37][22] for helping me find this resource!

## Conclusion

So there are plenty of solutions out there to this issue, and I am sure that I can use one of the approaches listed above to incorporate a working model and solution into my own application. Time for more reading, coding experiments, and hopefully a working application soon!

## Questions for the Future

Not researched this question yet&mdash;but if web applications that are run from users local machines are going to increase over time (I run a few myself already)&mdash;how are we going to manage the proliferation of port usage for all the local web servers running each application? Each web server, for each web application will need its own local port.

So, currently when a web application is written, it runs its own web server on an unprivileged port on the local machine (ie `http://localhost:8080/` where `8080` is the port). That port defaults to one chosen by the codes author. If we all pick the same ones, then the user is going to be left with the task of re-configuring the application and assigning a free port on their computer. 

So the question is: *is there a local web server port usage/allocation solution out there, or some kind of mux, or Application Programming Interface (API) we can use?*

As I said at the start of this article - Go has a 'modern awareness of the web', so does it already have a clever solution we can all adopt?

Otherwise&mdash;time to research local machine port scanning code to find a solution!


## Further Information

Further resources and information used in-conjunction with the above article (and linked to in-line) can be found here:

- http://golang.org/ 'The Go Programming Language' - @google
- http://mattn.github.io/go-gtk/ 'Go GTK' - @mattn
- https://github.com/visualfc/goqt 'GoQT' - @visualfc
- https://github.com/AndrewBC/cursed 'Cursed' - @AndrewBC
- https://github.com/go-qml/qml 'go-qml' - @go-qml
- https://github.com/nsf/termbox-go 'termbox-go' - @nsf
- https://github.com/syncthing/syncthing 'Syncthing' - @calmh
- http://getbootstrap.com/ 'Bootstrap open-sourced framework' - @mdo and @fat
- https://golang.org/doc/articles/wiki/ Golang.org wiki article: 'Writing Web Applications'- @google
- http://www.reinbach.com/ Greg Reinbach blog article 'Golang Web Apps'
- http://www.wiremoons.com/posts/2014-10-01-goLang-why-two-log-outputs 'Why Two Log Outputs?' - @wiremoons
- http://golang.org/pkg/html/template 'HTML Templates Package' - @google
- http://shadynasty.biz/blog/2012/07/30/quick-and-clean-in-go/ 'Quick and Clean in Go' - blog article by @zeebo
- https://github.com/jteeuwen/go-bindata go-bindata - *A small utility which generates Go code from any file.* - @jteeuwen
- https://github.com/GeertJohan/go.rice go.rice - *a Go package that makes working with resources such as html,js,css,images,templates, etc very easy.* - @GeertJohan
- http://mattjibson.com/blog/2014/11/19/esc-embedding-static-assets/ 'esc: Embedding Static Assets in Go' - blog by @mjibson
- https://github.com/mjibson/esc 'esc - *A simple file embedder for Go*' - @mjibson
- https://github.com/rakyll/statik 'statik - *Embed static files into a Go executable*'' - @rakyll
- http://syncthing.net/ 'Syncthing - *Syncthing replaces proprietary sync and cloud services with something open, trustworthy and decentralized*'
- http://www.sqlite.org/ SQlite - *SQLite is a software library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine.*
- http://golangweekly.com/ 'A great weekly 'Go Newsletter' sent to your inbox&mdash;once your subscribe of course!'' - @mattrco and @kelseyhightower
- http://www.golangweekly.com/archive/go-newsletter-issue-37/ Go Newsletter Issue #37


[1]: http://golang.org/ "'The Go Programming Language' - @google"
[2]: http://mattn.github.io/go-gtk/ "'Go GTK' - @mattn"
[3]: https://github.com/visualfc/goqt "'GoQT' - @visualfc"
[4]: https://github.com/AndrewBC/cursed "'Cursed' - @AndrewBC"
[5]: https://github.com/go-qml/qml "'go-qml' - @go-qml"
[6]: https://github.com/nsf/termbox-go "'termbox-go' - @nsf"
[7]: https://github.com/syncthing/syncthing "'Syncthing' - @calmh"
[8]: http://getbootstrap.com/ "'Bootstrap open-sourced framework' - @mdo and @fat"
[9]: https://golang.org/doc/articles/wiki/ "Golang.org wiki article: 'Writing Web Applications'- @google"
[10]: http://www.reinbach.com/ "Greg Reinbach blog article 'Golang Web Apps'"
[11]: http://www.wiremoons.com/posts/2014-10-01-goLang-why-two-log-outputs "'Why Two Log Outputs?' - @wiremoons"
[12]: http://golang.org/pkg/html/template "'HTML Templates Package' - @google"
[13]: http://shadynasty.biz/blog/2012/07/30/quick-and-clean-in-go/ "'Quick and Clean in Go' - blog article by @zeebo"
[14]: https://github.com/jteeuwen/go-bindata "go-bindata - *A small utility which generates Go code from any file.* - @jteeuwen"
[15]: https://github.com/GeertJohan/go.rice "go.rice - *a Go package that makes working with resources such as html,js,css,images,templates, etc very easy.* - @GeertJohan"
[16]: http://mattjibson.com/blog/2014/11/19/esc-embedding-static-assets/ "'esc: Embedding Static Assets in Go' - blog by @mjibson"
[17]: https://github.com/mjibson/esc "'esc - *A simple file embedder for Go*' - @mjibson"
[18]: https://github.com/rakyll/statik "'statik - *Embed static files into a Go executable*'' - @rakyll"
[19]: http://syncthing.net/ "'Syncthing - *Syncthing replaces proprietary sync and cloud services with something open, trustworthy and decentralized*'"
[20]: http://www.sqlite.org/ "SQlite - *SQLite is a software library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine.*"
[21]: http://golangweekly.com/ "'A great weekly 'Go Newsletter' sent to your inbox&mdash;once your subscribe of course!'' - @mattrco and @kelseyhightower"
[22]: http://www.golangweekly.com/archive/go-newsletter-issue-37/ "Go Newsletter Issue #37"