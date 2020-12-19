---
layout: project
title: "Image EXIF Scrubber"
github_url: "https://github.com/omkarmoghe/exif-scrubber"
languages:
  - Python
order: 3
---

**TL;DR** &mdash; Remove GPS and other metadata from photographs.

As the George Floyd protests erupt across the country, thousands of images of the events are flooding social media. On Twitter, many are pointing out that the faces of protestors should not be publicized to [protect their identities](https://www.google.com/search?q=ferguson+protestors+killed).

Along with what's visible in the photo, most images taken on modern cameras contain a range of metadata, encoded in the [EXIF](https://www.wikiwand.com/en/Exif) standard. This can include informational data like camera make/model, lens information, or even sensitive data like the exact latitude and longitude the image was taken at.

I believe that documenting events like these are important, but so is the privacy of everyone involved. This tool allows photographers to easily scrub EXIF data from a batch of images files and control which tags are removed.

You need a [Python 3.x](https://www.python.org/downloads/) installation to run the tool. Most computers ship with Python preinstalled. Run the tool from the command line with a glob pattern to match the image files you want to modify.

<pre><code class="shell">
$ python3 exif-scrub.py "~/Pictures/protest/*.jpg" --gps

- gps_latitude_ref
- gps_latitude
- gps_longitude_ref
- gps_longitude
- gps_altitude_ref
- gps_altitude
- gps_speed_ref
- gps_speed
...
</code></pre>

The scrubber will not modify your original images. A copy with the postfix "_SCRUBBED" will be saved in the same directory as the original file.
