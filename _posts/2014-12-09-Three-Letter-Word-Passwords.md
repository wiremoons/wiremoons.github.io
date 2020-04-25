---
title : "Three Letter Passwords"
description : "Exploring the use of three letter words to create a password"
published: true
layout: post
date : 2014-12-09
tags : [ "go", "golang", "password", "passgen","three word password" ]
categories : ["development", "golang", "go", "security" ]
---
## Three Word - Password Generator

## Summary

I recently created a simple application named uniquely as `passgen`
which was written to generate a random password created from a pool of
English three letter words. This gave me a small Golang project to
work on, and also to create a handy tool to generate strong passwords.

### How Does it Work?

This application generates password suggestions based on a pool of
several hundred three letter English words. The words are selected
from the pool randomly, and then displayed to the screen so you can
choose one for use as a very secure password.

It is important that you combine the three letter words together to
form a single string of characters (without the spaces)&mdash;to
obtain a password with a minimum length on 9 characters. Longer
combinations are stronger, but unfortunately not all sites or computer
systems accept really long passwords still.

You can of course add digits/numbers to your password also, and
punctuation characters too if you wish&mdash;but it would be wiser to
keep the password simple, and easy to remember, but change it more
frequently instead, using a fresh newly generated one every few weeks.

An example of the output from the program is below:
```
            THREE WORD - PASSWORD GENERATOR
            ¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯
• Number of three letter words available in the pool is: 573
• Number of three letter words to include in the suggested password is: 3
  • Password character length will therefore be: 9
• Mixed case passwords to be provided: false
• Offering 3 suggested passwords for your consideration:

   din wry ran
   ova ram lit
   ski yea koa

All is well
```

### Are These Passwords Secure?

While the passwords generated look far too simple and easy to be
secure, they are in fact very secure, and difficult to crack. Just
because they look simple to a human&mdash;it doesn't mean they are
simple to work out using a computer. They are in fact quite hard to
work out for a computer, as they are random, not a single dictionary
word, or a single common name (with perhaps number substitutions and
other common 'complex' combinations)&mdash;or some combination of
these approaches.  It is a common misconception that a password has to
be 'complex' to be any good.

Unfortunately we have been led to believe that the more complex a
password is&mdash;the better and more secure it will be&mdash;which is
in fact wrong.

In fact a longer password, but one that can that can be more easily
remembered, and therefore consequentially be changed more frequently,
actually offers a far greater degree of security.

For more information or more detailed explanations of this approach,
please see the web pages included below under 'References'. There are
plenty of other expert sources on the Internet also, that will explain
the security benefits of using a randomly generated three word (or
more) combination password. Just remember&mdash;your password must be
at least nine characters in total&mdash;or longer if possible. You can
of course always add additional punctuation, and/or numbers, should
you wish!

### So How Many Possible Passwords Are There?

There are over 500 three letter words in the pool that can be chosen
from, and assuming you use at least three of these words combined,
that provide 500^3 (500 to power of 3) possible combinations&mdash;of
which one is your password.

So - 500 x 500 x 500 = 125,000,000 (one hundred and twenty five
million) possibilities.

Maybe that doesn't sound like a lot - but if you could check 20 of
them every second, 24 hours a day, you would need roughly 60 days to
get through them all!

If you use the mixed case option (upper and lower case)&mdash;then
that number increases further of course&mdash;and you can still add
numbers, and/or punctuation characters if you wish too.

Or just increase you password length to 12 characters, so use four of
the three letter words combined, and you end up with 62,500,000,000 (sixty two
billion five hundred million) possibilities&mdash;and that just lower case
letters only.

### So Where Is It Then?

The [passgen source code](https://github.com/wiremoons/passgen) is available from my GitHub page, and I have made
pre-compiled binaries for Windows, Mac, and Linux available from a
[Release Page](https://github.com/wiremoons/passgen/releases). The application is not feature complete yet&mdash;but it
does work well, and will improve as I get time to work on it further.

### References

- Thomas Baekdal - The Usability of Passwords - FAQ
 - http://www.baekdal.com/insights/the-usability-of-passwords-faq
- Steve Gibson - GRC 'How Big is Your Haystack?'
 - https://www.grc.com/haystack.htm

