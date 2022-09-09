---
title: Fix Windows Security "Actions Recommended"
description: >
  Fix Windows Security "Actions Recommended" on Windows 11, when the dashboard
  shows all green checkboxes.
date: 2022-09-09T15:01:54-07:00
---

I had an issue today with Windows Security dashboard on Windows 11, where it
said there were "Actions Recommended" with an unnerving yellow warning icon,
however opening it up showed all green:

![Dashboard shows all green…](/img/blog/20220909-defender_green.png)

I looked at a lot of solutions which didn't fix the issue, but then found a
solution that I didn't find listed elsewhere so I'm writing it down: there's a
(maybe hidden?) pane of the Windows Security dashboard that isn't accessible
from the menu in the screenshot above, but you can find it via the Windows
"Settings" app:

![In Settings, click "Privacy & Security" followed by "Windows Security"](/img/blog/20220909-defender_settings_privacy.png)
![In Windows Security, click "App & browser control"](/img/blog/20220909-defender_settings_security.png)

Maybe it's hidden somewhere, but that "App & browser control" panel opens in the
Windows Security dashboard, yet I can't find it linked from within it. In there
I found the culprit: "Reputation Based Protection" was not turned on.

Re-enabling that cleared all recommended actions. Hopefully this helps you
because I just wasted an hour on it.
