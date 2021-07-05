---
title: Website settings
category: Configuration
order: 6
---

> \-\-index &lt;filename&gt;

Filename of the home page, defaults to `index.html`. This is useful if
- your web server expects another file name
- you already have an `index.html` and want the galleries to sit next to it

> \-\-albums-output-folder &lt;string&gt;

The default is for all HTML pages to sit at the root of the gallery.
This setting lets you group albums into a subfolder, so that the output looks like:

```
output
  |__ index.html
  |__ albums
  |   |__ holidays.html
  |   |__ birthdays.html
  |__ media
  |   |__ photo0001.jpg
  |   |__ photo0002.jpg
  |__ public
      |__ styles.css
```

This can help keep the output folder clean.
All relative links are still maintained for local browsing.
This setting does not affect the `index.html` page.

> \-\-theme &lt;choice&gt;

Specifies the theme to apply: `classic`, `cards`, `mosaic`, `flow`.
See the [themes](../../4-themes/themes) section for more information.

> \-\-theme-path &lt;path&gt;

Specifies the path to a custom local theme.
See the [creating a theme](../../4-themes/create) section for more information.

> \-\-theme-style &lt;path&gt;

Optional path to a CSS or LESS file.
This file is included after all other styles so you can override any particular style from the themes.

> \-\-theme-settings &lt;path&gt;

Optional path to a JSON file, used to configure specific theme options.

> \-\-title &lt;string&gt;

Website title. Defaults to "Photo album".

> \-\-footer &lt;string&gt;

Optional footer which can be displayed by the theme at the bottom of every page.
Can be either plain text or HTML.

> \-\-google-analytics &lt;string&gt;

Optional code for Google Analytics tracking.
