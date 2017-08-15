---
title: "Using Caps-Lock as a Compose key in XFCE4"
date: 2013-02-22T14:03:00-07:00
categories: [linux, xfce]
---

Why not take that useless key, and make it useful? The [Compose Key](http://en.wikipedia.org/wiki/Compose_key)—formerly present on many [Unix keyboards](https://www.google.com/search?q=sun+keyboard&hl=en&tbm=isch&tbo=u) of old—isn't present on most modern laptops.

In [XFCE](http://www.xfce.org/), this is a little more cumbersome than Gnome, since there isn't a graphical interface for `setxkbmap`, but these two commands will swap the Caps lock for a compose key:

```bash
setxkbmap -option ctrl:nocaps  # disables caps lock
setxkbmap -option compose:caps  # sets caps key to compose
```

To make this active at login, you can add two entries to your "Session and Startup" -> "Application Autostart" available under the **Settings Manager**. I've named mine "compose1" and "compose2", each containing one of the above commands. Now enjoy all those [en-dashes](http://en.wikipedia.org/wiki/Dash#En_dash) you'll surely be typing.
