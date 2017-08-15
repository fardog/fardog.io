---
title: primes, A Twitter-backed prime number generator
date: 2013-10-15T15:44:00-07:00
categories: [code, Javascript, projects, node.js]
---

[![_primes_ Twitter avatar](/img/blog/2013-10-16_primes.png)][1]

A few weeks ago I started [_primes_][1], a Twitter-backed [prime number][2] generator. When I say "Twitter-backed", I mean the bot that updates the account has very little idea of state: it only knows the last number it calculated because Twitter provides it. The program flow is this:

1. Start up
2. Fetch the most recently calculated prime from the Twitter feed.
3. Calculate the next prime.
4. Post if an hour has passed since the last posting. Wait if not, then post.
5. Steps 2–4 forever.

Technically steps two and four are merged after the first posting, since Twitter returns your latest tweet as a response when you post it. You can [view the code][3] on Github. It's written in CoffeeScript on Node.js, and uses [node-fibers][4] since I orginally built the prime number algorithm as a [generator][5], but it's unnecessary in this case.

The only state the application maintains is an internal "last posting" date, just as a fail-safe if the server it runs on spontaneously changed time: I didn't want it accidentally posting too quickly.

You should give it a follow if you love primes! Or maybe [fork the code][3] and make some new Twitter bots? I'd love to see some.

[1]: https://twitter.com/_primes_
[2]: http://en.wikipedia.org/wiki/Prime_number 
[3]: http://github.com/fardog/_primes_
[4]: https://github.com/laverdet/node-fibers
[5]: http://en.wikipedia.org/wiki/Generator_(computer_programming)
