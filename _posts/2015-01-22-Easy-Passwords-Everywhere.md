---
title : "Easy Passwords Everywhere"
description : "More password chaos revealed on the internet"
published: true
layout: post
date : 2015-01-22
tags : [ "c", "sugpass", "password", "passgen","three word password" ]
categories : ["development", "c language", "c", "security" ]
---
## Easy Passwords Everywhere

## Summary

The internet news of two days ago (20 Jan 2014) was all about the
terrible passwords being used in 2014 for protecting access to online
services. The news originated from a press release posted the password
management company SplashData.

The most commonly used top 10 poor passwords were:

1. 123456
2. password
3. 12345
4. 12345678
5. qwerty
6. 123456789
7. 1234
8. baseball
9. dragon
10. football

These passwords were obtained by looking at the reams of leaked
password lists (over 3 million passwords) that are available on the
internet, published following security breaches at various
companies. While the leaking of a persons password is unfortunate, it
does give an insight into the continued poor password choices being
made by individuals to protect their online assets.

## A Solution?

What is common to all the passwords shown in the list, is that they
are all easy to remember! This is probably why they were chosen in the
first place. People don't want to have complex passwords that they
can't easily remember, and have to look up constantly. It is a case of
convenience over risk.

Also as pointed out in an earlier post I made
[2014-12-09-Three-Letter-Word-Passwords](http://www.wiremoons.com/posts/2014-12-09-Three-Letter-Word-Passwords),
the requirements to make a password 'complex' are misunderstood.

A password should only be complex for a computer to break — but not
necessarily for a human to remember. These are two different
requirements often merged into one concept. 

This means not making it easy by using normal dictionary words(ie
password; qwerty; baseball; dragon; football) — as a computer can
check these extremely fast, making them pretty much useless. Also,
using the simple substitutions for letters such as 'p@55word' instead
should be avoided, as once again these are so well known, they can
easily be checked by a computer also, using the large dictionary word
list, and just tweaking it with these substitutions too!

So we need an easy to remember password without using a single dictionary
word?

Well humans aren't too good at remembering long numbers either, so how
about a selection of small words?

These small words together make up a nonsense word for a computer, and
therefore it has to check all combination to work the actual password
being used. However, passwords made up of small easy to remember words
are not too hard for us recall, and we are able to pick out the
individual words ([human pattern recognition is actual very very
good!](http://bigthink.com/endless-innovation/humans-are-the-worlds-best-pattern-recognition-machines-but-for-how-long))
— thus offering a much better password, and one that makes it easier
to remember as well.

You can have a look at two programs I have written —  both available from my GitHub account. 
One is written in c, and the other using Go. They both carry out the same 
task in offering a randomly generated password using a dictionary of over 1,000 three letter 
English words. Maybe they will help create a more secure password for you to use...  

They were written for fun, and for my own personal use, and they are both Open-source and free!

- [sugpass - 'Suggest Password - program written in c'](https://github.com/wiremoons/sugpass)
- [passgen - 'Password Generate - program written in Go'](https://github.com/wiremoons/passgen)


## Referenced Sites

A few of the sites with 2014 common password lists and news articles:

- [splashdata - '"Password" unseated by "123456" on SplashData's annual "Worst Passwords" list'](http://splashdata.com/press/worstpasswords2013.htm)
- [Time - 'These are the 25 worst passwords of 2014'](http://time.com/3672431/worst-passwords/)
- [Mashable Uk - 'Worst passwords of 2014 are just as terrible as you'd think'](http://mashable.com/2015/01/20/worst-passwords-of-2014/)
