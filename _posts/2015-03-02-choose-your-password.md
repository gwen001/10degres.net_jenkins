---
title: Choose your password
tags:
- password
- security
see_also:
  -
    link: A good article about how to test your password
    url: http://www.makeuseof.com/tag/test-password-strength-tool-hackers-use/
---
I recently worked with a well known web agency in France. 
They have a good reputation, they were rewarded last year for their good works and they are in the top 40 of the best french agencies. 

However I was terribly surprised to find many basic errors/misconfiguration on their own site: error_reporting enable, 
SQL injection and finally a "private" admin section reachable with a simple couple of `demo`/`demo` as credentials...

Such vulnerability can be dangerous when using common login/password and it can be even deadly if the discovered user has high privileges. 
It was true in this situation: mail contact, articles, resumes, photos everything was alterable.

Below the good practice to create a strong password.

## The rules

- must be at least 8 characters
- must be different than your previous password
- must NOT be related to your username
- must NOT contain any recognizable word

<!--more-->

## The characters set

- must contains uppercase: `A, B, C, ...`
- must contains lowercase: `a, b, c, ...`
- must contains number: `0, 1, 2, ...`
- must contains symbol: `#, §, %, @, &, ...`

## The method

A good approach is to choose a random sentence, a song title, a proverb, a book excerpt or whatever you'll remember easily... Extract the first letter of each words, apply some changes and add some chars.

## The example

I choosed a small sentence from "Fade into you" by Mazzy Star:

1. the original: `Some kind of night into your darkness`
2. first letters: `skoniyd`
3. replace letters by numbers: `sk0n1yd`
4. transform lowercase to uppercase: `Sk0n1yD`
5. add symbols to reach a good length: `#Sk0n1yD*`

An attack based on a dictionary would fail against this password. A brute force still possible but will be much much loooooonnnnnnger :)
