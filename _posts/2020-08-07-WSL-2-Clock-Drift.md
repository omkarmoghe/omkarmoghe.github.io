---
layout: blog_post
title: "WSL 2 Clock Drift"
date: 2020-08-07
tags:
  - wsl
  - linux
---

I noticed something strange in my `git log` today while working. Someone had apparently time traveled into the future and pushed some code.

![Future commits](https://i.imgur.com/w8HBG8i.png)

Weird. I figured it was probably something to do with time zones and didn't think much of it. A couple hours later, I noticed another timing related issue when running `sudo apt update`.

```
E: Release file for http://archive.ubuntu.com/ubuntu/dists/focal-updates/InRelease is not valid yet (invalid for another 1d 2h 44min 21s). Updates for this repository will not be applied.
E: Release file for http://archive.ubuntu.com/ubuntu/dists/focal-backports/InRelease is not valid yet (invalid for another 1d 2h 44min 44s). Updates for this repository will not be applied.
```

Even weirder. It seemed like my Ubuntu clock was behind "realtime", even though my Windows clock was accurate. Some Google-ing led to the discovery of an existing issue in WSL 2 which causes the Linux distro's clock to drift when the PC is asleep or hibernating.

- [system date is not same with windows (WSL 2) #4245](https://github.com/microsoft/WSL/issues/4245)
- [Release file for repository is not valid yet](https://github.com/microsoft/WSL/issues/4114)
- [WSL2 date incorrect after waking from sleep #5324](https://github.com/microsoft/WSL/issues/5324)

There were a couple suggested solutions to this that seemed to work, including shutting down WSL (`wsl --shutdown`). Alternatively, you can set the system time from the hardware clock (presumably Windows in this case) which doesn't require shutting down WSL:

```bash
sudo hwclock -s
```

My terminal prompt which prints the execution time of the last command confirmed that did the trick. The clock fast forwarded to realtime, effectively taking a day for the command to complete.

![](https://i.imgur.com/yBAdcQn.png)

[Clock drift](https://www.wikiwand.com/en/Clock_drift) is kind of scary because it's often hard to detect directly, and is usually observed via its side effects (e.g. future commits, invalid release files).

I'm a fan of the Windows Subsystem for Linux, and I hope they get this resolved soon.
