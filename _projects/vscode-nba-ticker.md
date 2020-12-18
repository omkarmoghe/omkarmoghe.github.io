---
layout: project
title: "VSCode NBA Ticker"
github_url: "https://github.com/omkarmoghe/NBA-Ticker"
languages:
  - Typescript (VSCode Extension)
order: 4
---

TL;DR: [NBA scores in your VSCode status bar](https://marketplace.visualstudio.com/items?itemName=omkarmoghe.nba-ticker).

![NBA Ticker demo](https://imgur.com/9UBEw9f.gif)

![NBA Ticker detail popup](https://imgur.com/dW8HKoh.png)

I switched over to VSCode from Sublime sometime in early 2020 and have been itching to write an extension ever since. Hackable editors are one of my favorite things, mainly because you don't have to sit around hoping new functionality will be added. A majority of my time working is spent in my editor, so naturally I decided to build an extension to lower my productivity.

I wasn't sure what I wanted to build when I started, so I worked backwards from this list of [public APIs](https://github.com/public-apis/public-apis). My original idea for this extension was a more general purpose status bar extension that could display arbitrary information in a marquee effect a la news broadcasts. My initial plan was to display market data, specifically stock quotes, but I quickly realized that getting said data was (in one way or another) too expensive.

After 86-ing my poor man's Bloomberg terminal, I came across the [balldontlie API](https://www.balldontlie.io/#introduction). I'm a huge basketball fan so building an extension to track games and keep me updated on scores was a perfect way to mess around with VSCode's extension API. The best part was that balldontlie was truly public, which meant I didn't need to distribute keys or do the annoying OAuth dance to hook my app (in this case an extension) to the API. Big shout out to the maintainers of that project.

Anyways, the extension contributes a small section in your status bar that cycles through the day's games.

![Status bar screenshot](https://i.imgur.com/X3ztw8Q.png)

Picking a format to succinctly deliver the score was hard, so I added settings for customizing how the ticker is displayed. You can also filter by teams, and set the poll frequency cycle speed. For example:

![Status bar screenshot 2](https://i.imgur.com/pXTZVSo.png)

The project will be maintained on GitHub; refer to the repo for a more detailed README, changelog, and configuration options. Feel free to open issues or pull requests!
