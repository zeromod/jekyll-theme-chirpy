---
layout: post
title: Killing Alt + Tab
date: 2020-07-23 08:56 +0530
---

Systems were not smart enough to perform multiple tasks simultaneously, then appears the greatest invention of [Parallel programming](https://en.wikipedia.org/wiki/Parallel_programming_model#:~:text=In%20computing%2C%20a%20parallel%20programming,and%20their%20composition%20in%20programs.) with supporting technologies like multiple cores. This  provides the Operating Systems to run multiple programs simultaneously.  Processors have become more powerful and this also helped Operating Systems provide more functionality to the users. Technology is evolving rapidly, where complex task can be executed with less efforts. 

### The Problem

The ability to run multiple programs simultaneously is backed by [Window Manger](https://en.wikipedia.org/wiki/Window_manager#:~:text=A%20window%20manager%20is%20system,help%20provide%20a%20desktop%20environment.) (one of the system software). Most Operating Systems provides their own implementation of *Window Manager*, like famous [X window system](https://en.wikipedia.org/wiki/X_Window_System) for UNIX based operating systems.

User Inputs of a typical computing systems are *Keyboard* and *Mouse*. While both of them excels at their use cases, keyboards provide more functionalities. Performing multiple tasks simultaneously by a user requires more effort. The main problem here is *Context Switching* (switching between programs). It should be seamless, less distracting and convenient.

While most of the window managers provides various functionality, they are targeted at common users (*User-Friendly*). Which is obviously not suitable for power users. The biggest pain for power users is ***Alt + Tab***, a keybinding (keyboard shortcut) for switching between windows (programs). The *Context Switching* here is really heavy, distracting and is counter productive.

### The Solution

One of the popular solution is [Tiling window manager](https://en.wikipedia.org/wiki/Tiling_window_manager), a tool which arranges windows in you're workspace automatically. But it will take time to get used to it and needs practice.

Another solutions to this problem is actually simpler than it looks, with these great tools

- [AutoKey](https://github.com/autokey/autokey) for Linux based systems.
- [AutoHotKey](https://github.com/AutoHotkey/AutoHotkey) for Microsoft Windows.

***Custom keybinding***  the programs.

for example `RAlt + c`  key binding executes following tasks

- *Switch* to Chrome window (If a chrome instance is running)
- *Launch* Chrome (if a chrome instance is not running) and switch to Chrome window
- *Cycle* through Chrome window (if multiple instance of chrome is running)

All the above 3 tasks (launch, switch, cycle) with a single key binding

Getting started to these tools is pretty simple, it took me ~10 minutes to setup keybindings for my daily usage programs (8 programs totally).

I am using Ubuntu as my daily driver, so moving forward, we will be looking more into [AutoKey](https://github.com/autokey/autokey).

### Autokey setup

Installing Autokey is pretty straight forward and have detailed [documentation](https://github.com/autokey/autokey/wiki/Installing#contents). Once the setup is complete, you can use the GUI to create the scripts. With Autokey every script is itself a python program, so if you're already familiar with python you can do wonders with Autokey, or if you're like me, new to python you can get started with writing script for switching between windows pretty easily.

Start writing scripts and attach a *Hot Key* to your script.

We will be also using autokey + [jumpapp](https://github.com/mkropat/jumpapp) for window switching, jumpapp is a simple wrapper around [wmctrl](http://tripie.sweb.cz/utils/wmctrl/).

### Showcase

*Note : All these showcase's outcome can be achieved also via [AutoHotKey](https://github.com/AutoHotkey/AutoHotkey)*

#### Window switch

Switching between windows is a simple script with just one line command, most of the heavy work is carried over by jumpapp

```python
command = 'jumpapp -r google-chrome'
system.exec_command(command, getOutput=False)
```

The above command will

- *Switch* to Chrome window (If a chrome instance is running)
- *Launch* Chrome (if a chrome instance is not running) and switch to Chrome window
- *Cycle* through Chrome window (if multiple instance of chrome is running)

These same command can be used for any programs by simply changing the name of the program in the command. 

*Note : these names are not usually what you see in the window title or application name. To get the program name you can run the command `wmctrl -lx` in you're terminal which lists all the currently opened windows and its name.*

#### Global google search

Being able to search any selected text by activating a python script (below) everytime I pressed a set of keyboard buttons.

```python
import webbrowser
base="http://www.google.com/search?q="
phrase=clipboard.get_selection()

#Remove trailing or leading white space and find if there are multiple 
#words. 
phrase=phrase.strip()
singleWord=False
if phrase.find(' ')<0:
    singleWord=True

#Generate search URL. 
if singleWord:
    search_url=base+phrase
if (not singleWord):
    phrase='+'.join(phrase.split())
    search_url=base+phrase

webbrowser.open_new_tab(search_url)
```

*[credits](https://github.com/autokey/autokey/wiki/Scripting#googling-query-from-anywhere)*

These can be useful in many ways like getting meaning for a word, google search on error messages, etc.

#### Typora

I like when I can see the preview of the markdown I am writing, which most of the programs lacks really bad, like [Trello](https://trello.com/en). So this simple autokey script solves this issue by launching the content in [typora](https://typora.io/) for me,

```python
snip = clipboard.get_selection()

file_path_cmd = 'echo "$HOME/tmp/typora-temp.md"'
file_name = system.exec_command(file_path_cmd, getOutput=True)


file = open(file_name, 'w+')
file.write(snip)
file.close

typora_cmd = 'typora ' + file_name
system.exec_command(typora_cmd, getOutput = False)
```

*I use Typora to write these blogs :smile:*

#### Jitsi

We use [jitsi meet](https://jitsi.org/jitsi-meet/) intensively for having meetings, pair programming and discussions. It is a web app, were need to launch a web browser and enter the link to join the room. We use jitsi daily and it was repetitive task to getting into the room, so we have written a autokey script to simply the process.

```python
import webbrowser

def open_jitsi(url):
    webbrowser.open_new_tab(url)
    clipboard.fill_clipboard(url)


jitsi_meet="https://meet.jit.si/"

rooms = [
    'room1',
    'room2',
    'room3',
    'others'
]
roomsCode, room = dialog.list_menu(
    options = rooms,
    title = 'Jitsi meet',
    message = 'Rooms',
    default = rooms[0],
    width = '300',
    height = '350'
)

if not roomsCode:
    if(room == rooms[-1]):
        roomInputCode, inputRoom = dialog.input_dialog(
            title='Jitsi meet',
	        message='Enter room', 
	        default= rooms[0]
        )
        if not roomInputCode:
            open_jitsi(jitsi_meet + inputRoom)
    else:
        open_jitsi(jitsi_meet + room)
    
```

![jitsi-blog](/assets/img/posts/jitsi-blog.gif)

#### scrcpy

Being an android developer, I will be testing the apps in an real device very often and [scrcpy](https://github.com/Genymobile/scrcpy)  is a tool which casts the screen you're system and lets you control the device from the system.

This script is pretty advanced where, it will launch scrcpy window for devices one by one on key press, and cycles through the windows if all the devices have a scrcpy window ready.

```python
def scrcpy(device_serial):
    return f'scrcpy --shortcut-mod=lctrl+lalt --window-x 1460 --window-y 150 --window-width 365 --window-height 730 --window-title scrcpy-{device_serial} -Sw --always-on-top -s {device_serial}'

def devices(device):
  return device.split('\t')[0]

adb_devices = "adb devices"  
adb_devices_output = system.exec_command(adb_devices).split('\n') 

adb_devices_output.pop(0)  #removing 'List of devices attached'
adb_devices_output.pop() #removing 'last emoty line

devices = list(map(devices, adb_devices_output))

wmctrl = "wmctrl -lx"
wmctrl_output = system.exec_command(wmctrl, getOutput=True)

for device in devices:
    if device not in wmctrl_output:
        system.exec_command(scrcpy(device), getOutput=False)
        break

command = 'jumpapp -r scrcpy' 
system.exec_command(command, getOutput=False)
```

Remember the `wmctrl -lx` command we used to get windows list, here we have more use cases for the command.![scrcpy_switch](/assets/img/posts/scrcpy_switch.gif)