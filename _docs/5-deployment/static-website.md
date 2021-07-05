---
title: Static website
category: Deployment
order: 1
---

Thumbsup is a fully static generator. The output is fully self-contained, and uses relative links.
This means it can be browsed from disk in your favorite browser, or hosted on any simple web server.
There is no application server needed.

### Generated gallery structure

The generated static website has the following structure:

```bash
website
  |
  |   # all albums
  |   # which are just HTML pages
  |__ index.html
  |__ sydney.html
  |__ paris.html
  |
  |   # files to support the website
  |   # e.g. scripts, css...
  |__ public
  |
  |   # your photos and videos
  |   # including resized versions and thumbnails
  |__ media
```
