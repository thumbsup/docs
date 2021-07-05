---
title: Output settings
category: Configuration
order: 4
---

> \-\-thumb-size &lt;integer&gt;

Size of the square thumbnails in pixels. Defaults to `120`.

> \-\-large-size &lt;integer&gt;

Height of the fullscreen photos in pixels. Defaults to `1000`.

> \-\-photo-quality &lt;integer&gt;

Quality of the resized images from `0` (worst) to `100` (best).
Default is `90`.

> \-\-video-format &lt;choice&gt;

Format of the resized videos, either `mp4` (default) or `webm`.
Note that encoding as `webm` is a lot slower.

> \-\-video-quality &lt;integer&gt;

The default behaviour is to use CRF (constant rate factor) to control the output quality.
This setting controls the quality between `0` (worst) to `100` (best). Default is `75`.

Notes:

- the quality scale is not linear
- you will most likely want a value between 50% and 90%
- values over 90% can generate files larger than the original

Here is an example of the quality setting used on a 35MB movie clip:

![Quality size ratio](../../images/video-quality.png)

> \-\-video-bitrate &lt;integer&gt;

Instead of CRF, you can specify a variable bitrate (a.k.a. average bitrate, or target bitrate).
Check the [ffmpeg docmentation](https://trac.ffmpeg.org/wiki/Encode/H.264) for more information.
This is not compatible with the `--video-quality` option.

Example: `--video-bitrate "1200k"`.


> \-\-photo-preview / \-\-video-preview / \-\-photo-download / \-\-video-download

These settings control the generation of the large "lightbox" photos and videos, and what happens when the user clicks "download". It also controls whether the original photos/videos are copied to the output folder.

All 4 settings default to `resize`. This means a web-friendly version is generated for all photos and videos, used both in the lightbox preview *and* for the download link.

| Value	| Behaviour |
|-------|-------------------------|
| `resize` | Generates and uses web-friendly versions in the gallery's media folder. |
| `copy` | Copies all original files into the output media folder, and points to them. This allows the gallery to be full-sized and self-contained, but it increases the size significantly. |
| `link` | Uses a simple relative link into the input folder. Does not generate or copy any files. |
| `symlink` | Creates symlinks to the originals in the output folder. This can be useful for galleries made for local browsing |

For example, given a photo called `MyAlbum/IMG_0001.jpg`:

```bash
--photo-download resize
# points download link to "large/MyAlbum/IMG_0001.jpg"

--photo-download copy
# points download link to "original/MyAlbum/IMG_0001.jpg" (which is created as part of the build)

--photo-download symlink
# points download link to "original/MyAlbum/IMG_0001.jpg" (which is a symlink to the input photo)

--photo-download link
# by default, will use a link to the photo in the input folder, relative to the output folder
# the path prefix can be overridden using the --link-prefix option
```

> \-\-link-prefix &lt;string&gt;

This settings controls the links when the settings above are set to `link`.
The default value is the relative path from the output folder to the input folder.
For example:

```bash
thumbsup --input /docs/photos --output /docs/website --photo-download link
# given a photo called holidays/IMG_0001.jpg
# the download link will point to ../photos/holidays/IMG_0001.jpg
```

This can be overridden with any relative or absolute path, or a URL. For example:

```bash
--photo-download link --link-prefix "../../"
# points download link to "../../holidays/IMG_0001.jpg" (which is assumed to already exist)

--photo-download link --link-prefix "https://static.mygallery.com/originals/"
# points download link to "https://static.mygallery.com/originals/holidays/IMG_0001.jpg" (which is assumed to already exist)
```

> \-\-cleanup &lt;boolean&gt;

When enabled (`--cleanup true`) this will generate the website as usual,
but also delete any **output** media files that are no longer referenced.

For example if you delete a photo from the **input** folder,
it will automatically delete the corresponding thumbnail since no album refers to it anymore.

*Note:* this never deletes any files from the input folder itself.

> \-\-concurrency &lt;integer&gt;

This controls the number of photos and videos processed in parallel.
The default value is the number of CPU cores on the current machine.
It might make the machine / server less responsive, in which case you can specify a fixed number:

```bash
--concurrency 2
# process a maximum of 2 photos/videos in parallel
```

*Note:* This corresponds to the maximum number of `exiftool`, `ffmpeg` and `graphicsmagick`
processes that are run at the same time, in addition to the main `Node` process.
There are other limiting factors in terms of performance, such as disk I/O,
so using more cores does not always increase performance.

> \-\-gm-args &lt;string&gt;

Extra command-line arguments to pass to [GraphicsMagick](http://www.graphicsmagick.org/) when processing images.

- `--gm-args` must be specified once per _logical_ option
- each option should be specified as a string, including all relevant arguments

For example:

```bash
# Equalise the image
thumbsup --gm-args 'equalize'

# Add a border around the image
thumbsup --gm-args 'border 5x5'

# Sharpen the image
thumbsup --gm-args 'unsharp 2 0.5 0.7 0'

# Sharpen and add brightness
thumbsup --gm-args 'unsharp 2 0.5 0.7 0' --gm-args 'modulate 120'
```

Note that post-processing will only apply to new images.
As with other options, thumbsups will not regenerate existing images because of a change of flags.

> \-\-watermark &lt;path&gt;

Overlays a watermark on all the resized images. The provided image should be a PNG with transparency.
The watermark does not affect downloadable images if `--download-photos` is set to `copy`, `link` or `symlink`.

```bash
thumbsup --watermark copyright.png
```

> \-\-watermark-position &lt;choice&gt;

Defines where the watermark is placed on the image. The possible values are:

- `Repeat`: the watermark is tiled across the entire image
- `Center`: the watermark is placed in the center of the image
- `NorthWest`,`North`,`NorthEast`,`West`,`East`,`SouthWest`,`South`,`SouthEast`: the watermark is positioned along the edges

```bash
thumbsup --watermark copyright.png --watermark-position Repeat
```

![watermark](../../images/watermark.jpg) ![tiled](../../images/watermark-repeat.jpg)
