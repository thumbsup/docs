---
title: Album settings
category: Configuration
order: 5
---

> \-\-albums-from &lt;string&gt;

As detailed in the [intro section](../../1-introduction/concepts), albums are a virtual concept in thumbsup.
Files can belong to arbitrary albums regardless of where they are on disk.
This setting determines how the photos and videos are grouped into albums.

The simplest value is a raw string where `/` denotes a hierarchy of albums. For example,
`--albums-from "Holidays/Tokyo"` specifies that all files should go in an album called `Tokyo`,
inside an album called `Holidays`.
A more typical use case is to use keywords to generate albums on the fly based on the photos being processed.

##### Folders

The value `%path` automatically expands to the nested folder structure, relative to the input folder.
For example:

| File path | Pattern | Value |
|-----------|---------|-------|
| `Holidays/Tokyo/IMG_0001.jpg` | `%path` | `Holidays/Tokyo` |
| `Holidays/Tokyo/IMG_0001.jpg` | `Family/%path` | `Family/Holidays/Tokyo` |

##### Date

The value `{format}` automatically expands to the date the photo/video was taken.
The format must be a valid [moment.js format](https://momentjs.com/docs/#/displaying/format/).

| File date | Pattern | Value |
|-----------|---------|-------|
| `2017-10-21` | `{YYYY}` | `2017` |
| `2017-10-21` | `{YYYY/MM}` | `2017/10` |
| `2017-10-21` | `{YYYY}/{MM - MMM}` | `2017/10 - October` |
| `2017-10-21` | `Years/{YYYY}/{MM}` | `Years/2017/10` |

This always uses the creation date if available in the <code>EXIF</code> data.
If there is no EXIF data, it tries to infer the date from the filename, or defaults to the file's <code>mtime</code>.
You can change `mtime` with many tools such as Unix's <code>touch</code> command.

##### Keywords

The value `%keywords` expands to every ITPC keyword present in the photo.
For example, if a photo is tagged with `sunset` and `beach`:

| Keywords | Pattern | Albums |
|-----------|---------|-------|
| `sunset`, `beach` | `%keywords` | `sunset`, `beach` |
| `sunset`, `beach` | `Tags/%keywords` | `Tags/sunset`, `Tags/beach` |


##### Multiple albums

A file can belong to zero, one or many albums.
On the command line, simply specify the flag multiple times:

```bash
thumbsup --albums-from "Folders/%path" --albums-from "Years/{YYYY}"
```

If using a config file, specify the patterns as an array:

```json
{
  "albums-from": ["Folders/%path", "Years/{YYYY}"]
}
```

##### Custom function

For more complex album structures, you can also specify a fully custom mapper
using `file://` followed by a file path (relative to the current directory).

```bash
--albums-from "file://mapper.js"
```

- The JavaScript file should export a single function which will be called for every file.
- The function must return an array of album names.
- The album names can include special tokens such as `%path` or `{YYYY}`.
- If you return an empty array, the photo will still be resized but won't appear in any albums

For example you can separate photos and videos:

```js
module.exports = file => {
  return file.meta.video ? ['Videos'] : ['Photos']
}
```

Or you can put all 5-star photos into a "Best photos" album:

```js
module.exports = file => {
  return (file.meta.rating === 5) ? ['Best photos'] : []
}
```

Or have special logic based on your directory structure:

```js
// if your folders are called "2016 [this event]"
const eventRegex = /(\d\d\d\d) \[([a-z\s]+)\]/
module.exports = file => {
  const match = eventRegex.exec(file.path)
  if (match) {
    const year = match[1]
    const event = match[2]
    return [`Events/${year}/${event}`]
  } else {
    return ['Events/Unsorted']
  }
}
```

You can either group all logic into a single function, or specify many mappers such as:

```bash
--albums-from "Photos/{YYYY}" --albums-from "file://5star.js" --albums-from "file://events.js"
```

> \-\-sort-albums-by &lt;choice&gt;

| Criteria | Detail |
|----------|--------|
| `title`  | Sort alphabetically by album title |
| `start-date` | Sort by the date of the earliest picture in the album |
| `end-date` | Sort by the date of the latest picture in the album |

Defaults to `start-date`.

> \-\-sort-albums-direction &lt;choice&gt;

Album sorting direction, either `asc` or `desc`. Defaults to `asc`.

> \-\-sort-media-by &lt;choice&gt;

How to sort photos and videos inside each album.

| Criteria | Detail |
|----------|--------|
| `filename` | Sort all media alphabetically by filename |
| `date` | Sort all media by date. The date is taken from EXIF data if possible, and otherwise infered from the filename or the file's <code>mtime</code>. |

Defaults to `date`.

> \-\-sort-media-direction &lt;choice&gt;

Media sorting direction, either `asc` or `desc`. Defaults to `asc`.
