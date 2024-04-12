---
layout: project
title: "PortableExpressions"
github_url: "https://github.com/omkarmoghe/portable_expressions"
languages:
  - Ruby
emoji: 🍱
order: 7
---

**TL;DR** &mdash; [A simple and flexible pure Ruby library for building and evaluating expressions.](https://github.com/omkarmoghe/portable_expressions) Fully compatible with JSON.

I wrote this library because I needed a way to write math expressions that were fully JSON serializable for another project. Although that project got [sidetracked](https://www.commitstrip.com/en/2014/11/25/west-side-project-story), I decided to package and release the framework for building and serializing these expressions.

Because most (all?) math operators in Ruby are implemented as methods, what I really ended up building was a framework to express any procedure or function call.
