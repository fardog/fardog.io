---
title: "Docker Read-Only Volumes with SELinux"
date: 2018-01-27T15:12:15-08:00
---

If you use Docker on an [SELinux][]-enabled Linux distribution like [Fedora][],
you may have run into issues mounting host volumes, and are probably aware of
the `z` flag to modify the SELinux label:

```
docker run --rm nginx -v /var/www/letsencrypt:/var/www/letsencrypt:z nginx
```

What isn't made very clear from the [Docker docs][] is that you can specify
multiple flags separated with commas, say `ro,z` to specify SELinux labeling
_and_ read-only:

```
docker run --rm nginx -v /var/www/letsencrypt:/var/www/letsencrypt:ro,z nginx
```

[fedora]: https://getfedora.org/
[selinux]: https://en.wikipedia.org/wiki/Security-Enhanced_Linux
[docker docs]: https://docs.docker.com/engine/admin/volumes/volumes/
