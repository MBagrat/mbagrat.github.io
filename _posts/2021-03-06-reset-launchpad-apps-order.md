---
layout: post
title: "Reset Launchpad Apps Order"
author: bagrat
categories: [ macos ]
image: assets/images/reset-launchpad-apps-order/macos-launchpad.jpg
tags: [sierra, big-sur]
---

The Launchpad in macOS helps you access applications quickly.
If you decide you want to reset the Launchpad apps where they appear in their default order, you can follow these steps.

1. From the Finder, select `“Go”` > `“Utilities”` > `“Terminal“`.
2. Type the following command, then press “Enter“.

```
defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock
```

The Launchpad apps will then be refreshed to their default state.