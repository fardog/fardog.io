---
title: Projects
date: 2017-08-14T21:51:00-07:00
lastmod: 2024-06-30T09:24:00Z
description: A small list of noteworthy projects.
---

I've written a lot of things—all can be found on my [github][]—however these are
the ones that are of particular interest, or that I'm particularly proud of.

[github]: https://github.com/fardog

## Swapper

- **What?** A Windows utility for swapping your mouse buttons with one click
- **Where?** [github.com/fardog/Swapper][swapper-github]
- **Why?** Swapping mouse buttons requires too many clicks

Swapper is a small tray utility which swaps your primary mouse button when
clicked, and gives a visual indication of the current primary button with the
tray icon. I use a mouse both left and right handed—depending on the device and
task—and having to open Windows Settings to switch was onerous.

The code is open source, or you can download a pre-built release from the
project's [releases page][swapper-releases]. It should work on any modern
version of Windows.

[swapper-github]: https://github.com/fardog/Swapper
[swapper-releases]: https://github.com/fardog/Swapper/releases

## primes

- **What?** A bot that posts prime numbers, sequentially; originally a Twitter
  bot, now elsewhere
- **Where?** [primes.today][] and [botsin.space/@primes][primes-mastodon]
- **Why?** Art?

The [original code][primes-original] for _primes_ is some of the worst I've
written; it was a CoffeeScript project, written before I decided to like
Javascript. It was also the first-ish thing I wrote for Node.js. But: one of the
most successful? 

(I later re-wrote it to run all bots from the [same codebase][primes-github].)

The idea behind primes was "twitter-as-database", but where it landed isn't
exactly that. Still; the social bots are completely stateless; it only knows
where it left off by retrieving its last posted prime number. To seed it, I
manually sent `2` from the respective platforms, and started the bot.

Before Twitter shut down API access, [the bot][primes-twitter] had amassed over
26,000 followers. Neat.

In advance of the inevitable shutdown, the bot was ported to a website:
[primes.today][].


[primes.today]: https://primes.today
[primes-twitter]: https://twitter.com/_primes_
[primes-mastodon]: https://botsin.space/@primes
[primes-original]: https://github.com/fardog/primebot
[primes-github]: https://github.com/fardog/_primes_

## frock

- **What?** A developer's tool for writing fake/mock services
- **Where?** [github.com/urbanairship/frock][frock-github]
- **Why?** Development environments can be painful; let's make them easier

_frock_ is one of the most useful things I've ever written. Urban Airship is a
microservice architecture, and the product I work on has the distinction of
needing to talk to the most services. _frock_ was written to keep my team's
development environment sane. Born out of previous tools that handled only small
aspects of our requirements, I set out to design a plugin-based tool that was
painless to write new fakes/mocks with, with [generic][frock-static]
[plugins][frock-proxy] for common needs.

It was written with Open Source in mind, so the [full commit
history][frock-commits] is available. I'm also particularly proud of the
[docs][frock-docs].

It's been a great help to my team, and I hope others find it useful.

[frock-github]: https://github.com/urbanairship/frock
[frock-static]: https://github.com/urbanairship/frock-static
[frock-proxy]: https://github.com/urbanairship/frock-proxy
[frock-commits]: https://github.com/urbanairship/frock/commits/master
[frock-docs]: https://github.com/urbanairship/frock/tree/master/docs

## secureoperator

- **What?** A DNS server for your local network which uses Google's
  [DNS-over-HTTPS][gdns] for lookups, masking DNS queries from your ISP
- **Where?** [github.com/fardog/secureoperator][secop-github]
- **Why?** DNS is not a secure protocol, this is one possible solution to the
  problem

_secure-operator_ is a DNS proxy server. What makes it different from usual DNS
proxies is that it performs all its DNS lookups over HTTPS, meaning they cannot
be snooped on by parties outside your local network. This is great if you don't
like the idea of your ISP tracking the websites you visit.

I mention it's one possible solution to the problem, [others][dnscrypt]
[exist][openresolve] but suffer from a different issue: unreliable servers, and
not necesarily in the "uptime" sense. You don't really know who's running these
other services either, so why not trust Google?

_secure-operator_ has been well received, made the rounds on a number of Golang
newsletters, and has been running reliably in my home for months. The number of
downloads it receives indicate it might be working well for others too.

[gdns]: https://developers.google.com/speed/public-dns/docs/dns-over-https
[secop-github]: https://github.com/fardog/secureoperator
[dnscrypt]: https://www.dnscrypt.org/
[openresolve]: https://www.openresolve.com/
