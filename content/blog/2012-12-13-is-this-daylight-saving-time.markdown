---
title: "Is this Daylight Saving Time?"
date: 2012-12-13T00:17:00-07:00
categories: [code, Python, Django, projects, zurb-foundation]
---

[Is this Daylight Saving Time?](http://isthisdst.us) is a single-page site to determine your DST status. You can check out the source on [GitHub](https://github.com/fardog/isthisdst/).

It's an overblown build—using [Django](http://djangoproject.com) for such a small site isn't necessary—but it was more of an exercise than anything. The implementation is my first attempt at some best practices, namely [fabric](http://fabfile.org) for automated deployment out of git, and [pipeline](https://github.com/cyberdelia/django-pipeline) for css/script minification.

Other packages used include [pytz](http://pypi.python.org/pypi/pytz) for timezone calculations and [boto](https://github.com/boto/boto) for automated upload of static assets to [S3](http://aws.amazon.com). [Zurb Foundation](http://foundation.zurb.com) was used for the frontend. 

Geolocation is done using the City and Country IP databases from [MaxMind](http://www.maxmind.com/en/geolocation_landing) and uses the Django-included [django.contrib.gis.geoip](https://docs.djangoproject.com/en/dev/ref/contrib/gis/geoip/) to read them. That resolves the lat/long, which I then send to [GeoNames](http://www.geonames.org/) for to get the timezone. That result is cached in a local SQLite database to minimize lookups to GeoNames.

Overall, it was a great learning experience. Like all great learning experiences it took entirely longer than I'd originally intended; all part of the fun.
