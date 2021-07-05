---
title: Miscellaneous
category: Configuration
order: 7
---

> \-\-config &lt;path&gt;

Maintaining many CLI arguments can be tedious.
You might want to save them somewhere and re-use them easily.
To help with this `thumbsup` supports passing arguments as a [JSON](https://en.wikipedia.org/wiki/JSON) file:

```bash
thumbsup --config config.json
```

Simply specify all arguments as an object, without the `--` prefix:

```json
{
  "input": "/media/output",
  "output": "./website",
  "title": "My holiday",
  "thumb-size": 200,
  "large-size": 1500,
  "download-photos": "copy",
  "download-videos": "large",
  "sort-albums-by": "date",
  "theme": "cards",
  "css": "./custom.css",
  "google-analytics": "UA-999999-9"
}
```

Flags such as `--cleanup` must be entered as a boolean value:

```json
{
  "cleanup": true
}
```

If an argument can be repeated on the command-line, you must specify it as an array in the JSON config:

```json
{
  "include": ["holidays/**", "events/**"]
}
```

Note that special characters such as quotes must be properly escaped:

```json
{
  "footer": "All images courtesy of <a href=\"https://unsplash.com\">unsplash.com</a>"
}
```

> \-\-log &lt;string&gt;

By default, thumbsup renders progress with a nice colorful output:

![Default output screenshot](../../images/default-output.png)

In case of errors, it should print the error message and stack trace to help troubleshoot.
You can print a full text-only log using `--log <value>`.

| Value | Description |
|-------|-------------|
| info | Basic text log, including stats and the files being processed |
| debug | More troubleshooting data like timestamp comparisons |
| trace | Full verbose mode, including the exact gm / ffmpeg commands being called |

When running outside of a TTY environment, thumbsup automatically defaults
to `--log info` if not specified.

If you face any issues, you can raise them at
[https://github.com/thumbsup/thumbsup/issues](https://github.com/thumbsup/thumbsup/issues).
Make sure to check existing issues first, and please provide as much detail as possible including:

- Operating system
- Node.js version (`node -v`)
- Thumbsup version (`thumbsup -v`)

> \-\-dry-run &lt;boolean&gt;

When specified, thumbsup will update the `thumbsup.db` database, but will **not** generate any resized media, nor generate the gallery pages.

> \-\-usage-stats &lt;boolean&gt;

By default, thumbsup reports anonymous usage stats such as the OS and the gallery size.
This is used to understand usage patterns and guide development effort, for example
"should we focus on Windows support" or "should we make optimizations for galleries of 10,000+ photos".

You can disable usage reporting by specifying `--no-usage-stats`.
When using a JSON config file you can set

```json
{
  "usage-stats": false
}
```

Rest assured that thumbsup will never report any personal data such as filenames,
album names or EXIF metadata. You can of course check the source code at
[https://github.com/thumbsup/thumbsup](https://github.com/thumbsup/thumbsup).
