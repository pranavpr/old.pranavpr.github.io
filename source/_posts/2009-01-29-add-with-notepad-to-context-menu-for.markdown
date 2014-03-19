---
layout: post
title: "Add 'Open with Notepad' to the Context Menu for All Files"
date: 2009-01-29
comments: true
categories:
 - Windows
 - Tricks
 - Notepad
---

I often use Notepad to open plain text files. If the extension of file is of unknown type then I have to browse a list of applications to open that file which is quite painful. Here is a small registry tweak which adds "Open with Notepad" to right click context menu for all file types.

## Manual Registry Tweak

Go to start menu and in search or run box type regedit and press enter to open registry editor. Now browse to following registry key:
{% codeblock %}
HKEY_CLASSES_ROOT\*\shell
{% endcodeblock %}

<div style="TEXT-ALIGN: center"><img src="/images/blog/openwithnp.png" /></div>

Right-click on "shell" and choose to create a new key "Open with Notepad". Create a new key below that one called "command". Double-click on the (Default) value in the right-hand pane and enter in the following:
{% codeblock %}
notepad.exe %1
{% endcodeblock %}

The change will take place instantaneously and you could see Open with Notepad option in right click context menu.

<div style="TEXT-ALIGN: center"><img src="http://lh3.ggpht.com/__GfZLYkxICU/SZ01PV6w-6I/AAAAAAAAKaA/buc8Owev7h0/%5BUNSET%5D.jpg?imgmax=800" /></div>

_**Disclaimer**: Modifying the registry can cause serious problems that may require you to reinstall your operating system. I cannot guarantee that problems resulting from modifications to the registry can be solved. Use the information provided at your own risk._
