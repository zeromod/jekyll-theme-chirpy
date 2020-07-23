---
layout: post
title: Killing Alt + Tab
date: 2020-07-23 08:56 +0530
---

Systems were not smart enough to perform multiple tasks simultaneously, then appears the greatest invention of [Parallel programming](https://en.wikipedia.org/wiki/Parallel_programming_model#:~:text=In%20computing%2C%20a%20parallel%20programming,and%20their%20composition%20in%20programs.) with supporting technologies like multiple cores. This  provides the Operating Systems to run multiple programs simultaneously.  Processors have become more powerful and this also helped Operating Systems provide more functionality to the users. Technology is evolving rapidly, where complex task can be executed with less efforts. 

### The Problem

The ability to run multiple programs simultaneously is backed by [Window Manger](https://en.wikipedia.org/wiki/Window_manager#:~:text=A%20window%20manager%20is%20system,help%20provide%20a%20desktop%20environment.) (one of the system software). Most Operating Systems provides their own implementation of *Window Manager*, like famous [X window system](https://en.wikipedia.org/wiki/X_Window_System) for UNIX based operating systems.

User Inputs of a typical computing systems are *Keyboard* and *Mouse*. While both of them excels at their use cases, keyboards provide more functionalities. Performing multiple tasks simultaneously by a user requires more effort. The main problem here is *Context Switching*, or switching between programs. It should be seamless, less distracting and convenient.

While most of the window managers provides various functionality, they are targeted at common users (*User-Friendly*). Which is obviously not suitable for power users. The biggest pain for power users is ***Alt + Tab***, a keybinding (keyboard shortcut) for switching between windows (programs). The *Context Switching* here is really heavy, distracting and is counter productive.

### The Solution

Solutions to this problem is actually simpler than it looks, with these great tools

- [AutoKey](https://github.com/autokey/autokey) for Linux based systems.
- [AutoHotKey](https://github.com/AutoHotkey/AutoHotkey) for Microsoft Windows.

***Custom keybinding***  the programs.

for example `Alt + c`  key binding executes following tasks

- *Switch* to Chrome window (If a chrome instance is running)
- *Launch* Chrome (if a chrome instance is not running) and switch to Chrome window
- *Cycle* through Chrome window (if multiple instance of chrome is running)

All the above 3 tasks (launch, switch, cycle) with a single key binding :scream_cat: 

Getting started to these tools is pretty simple, it took me ~10mins to setup keybindings for my daily use programs (8 programs totally :sweat_smile:).