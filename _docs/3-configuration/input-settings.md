---
title: Input settings
category: Configuration
order: 3
---


> \-\-include &lt;glob&gt;

By default, all files in the `--input` folder are added to the gallery.
This optional pattern controls which files to include instead.

- It must be a valid *glob* pattern such as `**/IMG_*`
- When targeting a folder name, make sure it's followed by stars, such as `holidays/**`
- If specified multiple times, files matching **any** of the patterns will be included
- This does not change the kind of extensions that are supported (`**/*.doc` will not include Word documents)

When using a JSON config file, make sure that this value is an array, such as

```json
{
  "include": ["**/IMG_*"]
}
```

> \-\-exclude &lt;glob&gt;

This optional pattern controls which files to exclude from the input folder.
Exclusions are applied after inclusions.

- It must be a valid *glob* pattern such as `**/IMG_*` or `**/work/**`
- When excluding a directory, make sure to use `folder/` or `folder/**` (not just the name)
- Some folders are always excluded by default, such as
  - any folder starting with a dot
  - Synology vendor folders (thumbnails, recycle bin)

When using a JSON config file, make sure that this value is an array, such as

```json
{
  "exclude": ["**/IMG_*"]
}
```

> \-\-include-photos &lt;boolean&gt;

Whether to index / process / display standard photo files (jpg, png...).
Notes:
- if set to `false`, the corresponding file types will be ignored regardless of any `--include` patterns
- if set to `true`, it doesn't bring back any files that were ignored using `--include/exclude`

> \-\-include-videos &lt;boolean&gt;

Whether to index / process / display standard video files (mp4, mov...).

> \-\-include-raw-photos &lt;boolean&gt;

Whether to index / process / display camera RAW files (Nikon NEF, Canon CR2...).
Just like other photos, RAW files are processed to generate thumbnails and a web-friendly preview.
Processing RAW files requires [dcraw](https://www.cybercom.net/~dcoffin/dcraw/) to be installed.

