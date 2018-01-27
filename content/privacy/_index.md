---
title: Privacy
date: 2018-01-01T14:16:00-08:00
---

Visitation statistics are collected for this site using [observe][], which I
use to understand how often pages are viewed, and from where. This data is
stored in a private [Google BigQuery][bq] table—to which only I have access—and
is removed after 90 days. The information in this table is:

* URL visited
* Anonymized Source IP address (IPv4 to 20 bits, IPv6 to 32)
* Time of visit
* HTTP headers received

[Observe][observe] itself runs on [Google App Engine][appengine], which logs
some information about the request. This information is kept for 7 days.

If your browser sends a [Do Not Track][dnt] header, I store no information about
the visit to BigQuery.

This site does not use [cookies][] directly, however Cloudflare may set some.

This site is served via [GitHub Pages][] and [Cloudflare][]. Refer to them for
any information or statistics they may collect.

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
