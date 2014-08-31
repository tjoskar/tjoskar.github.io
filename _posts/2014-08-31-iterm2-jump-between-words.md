---
layout: post
title: iTerm2 jump between words
date: 2014-08-31 10:18:10
published: True
categories: ['iTerm']
tags: []
---

To jump between words and start/end of line in iTerm2 follow these steps:

Click on the tab "Keys" in iTerm2 Preferences
Add the following Global Shortcut Keys:

* Jump to beginning of line
  * Keyboard shortcut: ⌘←
  * Action: Send Escape Sequence
  * Escape: [H
* Jump to end of line
  * Keyboard shortcut: ⌘→
  * Action: Send Escape Sequence
  * Escape: [F
* Jump to start of word
  * Keyboard shortcut: ⌥←
  * Action: Send Escape Sequence
  * Escape: b
* Jump to end of word
  * Keyboard shortcut: ⌥→
  * Action: Send Escape Sequence
  * Escape: f

Remove previous bindings (⌥← and ⌥→) (in Profiles -> Keys).


[That's all folks](https://www.youtube.com/watch?v=gBzJGckMYO4)
