---
title: Docker image
category: Installation
order: 3
---

Thumbsup is published as a Docker image in the GitHub package repository called
[thumbsup/thumbsup](https://github.com/thumbsup/thumbsup/pkgs/container/thumbsup).
It is available for amd64, arm/v7 and arm64.

To create a gallery, you will need to mount one or several folders so that the container can access:
- the input folder with photos & videos
- the output folder to write the gallery
- the config file, if using one

For example:

```bash
docker run -t              \
  -v "$(pwd):/work"        \
  -u $(id -u):$(id -g)     \
  ghcr.io/thumbsup/thumbsup \
  thumbsup --input /work/media --output /work/gallery
```

You can of course mount the volumes differently, for example:

```bash
docker run -t                   \
  -v /Volumes/photos:/input:ro  \  # the input folder can be read-only
  -v "$(pwd)/website:/output"   \
  -u $(id -u):$(id -g)          \
  ghcr.io/thumbsup/thumbsup      \
  thumbsup --input /input --output /output
```

*Notes:*

- the `-t` argument is for the progress bars to render properly
- the `-u` argument is to avoid permission issues, where the generated gallery is owned by an unknown user

Photo dates displayed on the website are based on the current machine timezone.
When running in Docker, this is `GMT`. If the timezone is important to you, you should also add

```bash
docker run -v /etc/localtime:/etc/localtime [...]
```

For more details about all the arguments and options available, see the [configuration](../../3-configuration/usage) page.
