---
title: "vim, fish shell, and vim-sensible"
date: 2013-03-22T00:07:00-07:00
categories: [linux, fish]
---

Part of this evening was spent making `vim` workable for me, the first step was getting some sane defaults. [vim-sensible](https://github.com/tpope/vim-sensible) was recommended in several places (as was Tim Pope's also excellent [pathogen](https://github.com/tpope/vim-pathogen)), but immediately after installation I was getting an error:

```
Error detected while processing ~/.vim/bundle/vim-sensible/plugin/sensible.vim:
line   75:
E484: Can't open file /tmp/v1NmKg8/0
Press ENTER or type command to continue
```

Frustratingly, line 75 held no obvious suggestions. After some time troubleshooting, I found it worked fine if I wasn't using [`fish` shell](http://www.ridiculousfish.com/shell/).

I'm not positive why this is the case, but you can fix the error by adding the following line to your `.vimrc`.

```
set shell=/bin/sh
```

On next launch of `vim`, all is resolved.
