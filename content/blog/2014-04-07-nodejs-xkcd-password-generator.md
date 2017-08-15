---
title: XKCD Password Generation for Node.js
date: 2014-04-07T17:30:00-07:00
categories: [code, Javascript, projects, node.js]
---

I created an [XKCD-style password generator for Node.js][github] (see [this comic][xkcd] for reference). You can find it [on my github][github] or do an `npm install -g xkcd-password` to get it on your path.

There are others available, but most don't include a CLI, or easily configurable options. Many use [small word lists][smallwordlist]. So I made one that uses a sufficiently large list—113,809 words provided by the [Moby Project][moby]—and both a simple commandline utility and a module that you can use in your Node.js apps. If you don't like my wordlist, you can substitute your own easily. Everything is non-blocking, so you shouldn't find it bogging down your stuff.

Send [issues][issues] or pull requests!

```
Usage: xkcd-password [options]

Options:
   -n, --numWords    The number of words to generate for your password.  [4]
   -m, --minLength   Minimum lengh of words chosen for the generated password.  [5]
   -x, --maxLength   Maximum length of words chosen for the generated password.  [8]
   -f, --wordFile    The newline-delimited list of words to be used as the source.
   -s, --separator   The separator character to use between words when output to the console.  [ ]
   --version         print version and exit
```

[xkcd]: http://xkcd.com/936/
[smallwordlist]: https://github.com/rstacruz/passwordgen.js/blob/master/lib/words.js
[moby]: http://icon.shef.ac.uk/Moby/
[github]: http://github.com/fardog/node-xkcd-password/
[issues]: http://github.com/fardog/node-xkcd-password/issues
