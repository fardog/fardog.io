---
title: "Simple 'workon' Command for fish shell"
date: 2013-02-15T00:15:00-07:00
categories: [code, fish, Python, virtualenv, virtualenvwrapper]
---

I put together a quick `workon` clone for the [fish's fish shell](http://www.ridiculousfish.com/shell/). Definitely nothing as complete or useful as [virtualenvwrapper](http://www.doughellmann.com/projects/virtualenvwrapper/), but it fixes my need: switching simply between virtualenv's and projects.

Installation
------------

First, create a function file `~/.config/fish/functions/workon.fish` with this defintion:

```
function workon -d "Activate virtual environment in $WORKON_HOME"
  set tgt {$WORKON_HOME}/$argv[1]
  if [ -d $tgt ]
    cd $tgt

    # deactivate any active venv, and activate the target
    # this needs to be rewritten with the `type` fish command
    if test -n "$VIRTUAL_ENV"
      deactivate
    end

    . bin/activate.fish

    if test (count $argv) -gt 1
      if test $argv[2] = "open"
        set -gx WORKON_OPEN_SUBLIME True
      end
    else
      set -ge WORKON_OPEN_SUBLIME
    end

    # open sublime text
    if test -n "$WORKON_OPEN_SUBLIME"
      open {$WORKON_SRC_DIR}/{$argv[1]}.sublime-project
    end

    # change to working dir
    if test -n "$WORKON_SRC_DIR"
      cd {$WORKON_SRC_DIR}/{$argv[1]}
    end
  else
    echo "$tgt not found"
  end
end

complete -c workon -a "(cd $WORKON_HOME; ls -d *)"
```

Then, define the necessary environment variables in your `~/.config/fish/config.fish` file:

```
set -gx WORKON_HOME ~/Documents/virtual_environments
set -gx WORKON_SRC_DIR ~/Documents/code
set -gx WORKON_OPEN_SUBLIME True
```

As you might imagine, this is set up for how I work, but it wouldn't be difficult to customize for your needs. Since this uses the virtualenv-provided `activate.fish` script, the `deactivate` command works as expected, as well as any other usual functions.
