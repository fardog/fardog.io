---
title: Privacy
date: 2018-01-01T14:16:00-08:00
---

This site is served via [GitHub Pages][] and [Cloudflare][]. Refer to them for
any information or statistics they may collect.

Visitation statistics are collected for this site using [observe][], which I
use to understand how often pages are viewed, and from where. This data is
stored in a private [Google BigQuery][bq] table—to which only I have access—and
is removed after 90 days. The information in this table is:

* URL visited
* Source IP address
* Time of visit
* HTTP headers received

[Observe][observe] itself runs on [Google App Engine][appengine].

If your browser sends a [Do Not Track][dnt] header, I store no information about
the visit. This site does not use [cookies][] directly, however Cloudflare may
set some.

To view how this site is built, see [its repository][repo] and
[this post][post], which details the setup.

[cookies]: https://en.wikipedia.org/wiki/HTTP_cookie
[Github Pages]: https://pages.github.com/
[Cloudflare]: https://www.cloudflare.com/
[observe]: https://github.com/fardog/observe/
[bq]: https://cloud.google.com/bigquery/
[dnt]: https://en.wikipedia.org/wiki/Do_Not_Track
[repo]: https://github.com/fardog/fardog.io
[post]: /blog/2017/08/26/automatically-publishing-websites-with-hugo-travis-and-github/
[appengine]: https://cloud.google.com/appengine/
