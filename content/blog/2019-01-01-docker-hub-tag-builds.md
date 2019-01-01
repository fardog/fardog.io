---
title: "Docker Hub Tag Builds"
date: 2019-01-01T12:59:11-08:00
---

[Docker Hub][hub] automatic builds are a great way to keep your Docker images
up to date with a `git` repository, but building sensibly tagged releases isn't
very straightforward. It's easy to create ones for a tag (like `v4.1.1`) but
less so to create versioned images, such as:

```
myimage:4
myimage:4.1
myimage:4.1.1
```

This means that when you release a `v4.1.2`, it becomes the new `4` and `4.1`.
It turns out that you can do this with docker automatic builds. At the time I
got this working it wasn't well documented, but these days it has [docs][].

> Note: This information was adapted from something I found in a Google search,
> but I've since lost the source. I'll update here if I find it again.

To achieve this, set up your build rules as follows:

| Type   | Name                                | Docker Tag Name |
|--------|-------------------------------------|-----------------|
| Branch | `master`                            | `latest`        |
| Tag    | `/^v([0-9.]+)$/`                    | `{\1}`          |
| Tag    | `/^v([0-9]+)\.([0-9]+)\.([0-9]+)$/` | `{\1}`          |
| Tag    | `/^v([0-9]+)\.([0-9]+)\.([0-9]+)$/` | `{\1}.{\2}`     |

Now, tag your versions in `git` as full semver specifiers, with a `v` in front,
e.g.:

```
git tag -a v4.1.2 -m 4.1.2
```

When your docker image is auto-built from git, it will now have tags which
match the example at the beginning of this post.

Eventually I'll write up a more thorough post on how you can perform
auto-builds and releases with github, docker hub, and travis. For now you can
learn a lot from this post, and by looking at the [travis config][travis] for
[secureoperator][].

[hub]: https://hub.docker.com/
[travis]: https://github.com/fardog/secureoperator/blob/master/.travis.yml
[secureoperator]: https://github.com/fardog/secureoperator/
[docs]: https://docs.docker.com/docker-hub/builds/#regexes-and-automated-builds
