---
title: "The New Yorker and the importance of optimization"
date: 2013-03-06T22:43:00-07:00
updated: 2013-03-07T09:58:00-07:00
categories: [code, optimization]
---

*Update 2013-03-07: Please see the correction at the bottom of this article.*

I have a small obsession web platforms, and discovering what platform a site is using. There are some [great tools](http://guess.scritch.org/) that help, but nothing beats [viewing the source](https://en.wikipedia.org/wiki/View-source_URI_scheme). Recently I encountered the *[New Yorker](http://www.newyorker.com/)*…

It's a sight. I encourage you to check the source for yourself, but it's a templating system gone wrong. The day I discovered it, I [tweeted](https://twitter.com/milkandtang/status/304454130448080897)—

> in the midst of 90 new-lines, sits @NewYorker's `<html>` tag. a scant 1006 lines later, the `<body>`.

—to no reply. It's for real though: their homepage serves, of this writing, *16101 newlines*. Many of their tags are surrounded by no less than 30 newlines each. Their `<head>` contains *998* alone. So this gets me thinking:

A newline character, usually represented as `\n` in many languages, but represented by the unicode `U+000A` tends to be represented as an 8-bit ASCII character, or one byte. For the purposes of this excercise we'll assume exactly that.

In terms of websites:

 - [The New York Times](http://nytimes.com) sends 1419 newlines.
 - [Apple](http://apple.com) sends 236
 - [Google](http://google.com) sends 87
 - [Reddit](http://reddit.com) sends 1 newline. One.

Now lets talk bandwidth: 16101 bytes is 0.0153 MB—less than two-hundredths of a megabyte—it's not much. Now traffic-wise: According to [Compete](http://compete.com), they see 1,103,088 unique visitors per month.

Or, if you prefer: **16.54 GB of newlines per month**, strictly in unique visits to the homepage. Hey, [Condé Nast](http://condenast.com): might be time to optimize?

**Correction:** Reddit has [bested me on this one](http://www.reddit.com/r/webdev/comments/19u4ho/the_new_yorker_and_its_16gb_of_newlines/). As the commenters point out, my math is done assuming that the web server isn't configured to compress with [gzip](http://en.wikipedia.org/wiki/Gzip)—and it is. Since [all major browsers support gzip](http://www.http-compression.com/), the actual impact of *The New Yorker's* excessive newlines is [minimal](http://www.reddit.com/r/webdev/comments/19u4ho/the_new_yorker_and_its_16gb_of_newlines/c8rc6ie).

So, this is more or less an article about the comical result of a templating system than it is about performance. Thanks to those on Reddit that pointed out this error.
