---
layout: project
title: "LA Metro Maps"
live_url: "https://omkr.dev/LA-Metro-Map"
github_url: "https://github.com/omkarmoghe/LA-Metro-Map"
languages:
  - HTML
  - Javascript
emoji: 🚇
order: 11
---

**TL;DR** &mdash; [A live map of LA's Metro light rail.](https://omkr.dev/LA-Metro-Map)

I love a good train system (shoutout [NUMTOTS](https://en.wikipedia.org/wiki/New_Urbanist_Memes_for_Transit-Oriented_Teens)), but though the LA Metro's [official, albeit beta, maps](https://livemap.metro.net/) lacked useful information like stop times and the integration of the schedule. I wanted to create a fast, but informative version with a UX optimized for mobile.

The data itself was annoying to stitch together, as many endpoints on the LA Metro API seem to not work. Other information like stop data and times had to be pulled from the GTFS Rail repository on GitLab and indexed for lookups. A tangential goal for this project was to use a minimal UI framework (Alpine) to render the map, train, and stop info. The live maps run entirely on your browser.

## OSS Software & Resources

- [LA Metro API](https://lacmta.github.io/metro-api-v2/docs/api)
- [LACMTA GTFS Rail](https://gitlab.com/LACMTA/gtfs_rail)
- [Metro Open Data](https://developer.metro.net/gtfs-schedule-data/)
- [OpenFreeMap](https://openfreemap.org/quick_start/)
- [Maplibre GL JS](https://github.com/maplibre/maplibre-gl-js/)
- [Alpine.js](https://alpinejs.dev/)
