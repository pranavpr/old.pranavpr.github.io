---
layout: post
title: "How to send message to users logged on to a UNIX terminal?"
date: 2009-05-07
comments: true
categories:
 - UNIX
 - Messaging
 - Linux
 - Tricks
---

One of the cool features of UNIX is that you don't need to have any software installed to send message to users logged on to that machine. There is a handy command called "write" which enables you to do this. Here is the syntax of this command:

{% codeblock lang:bash %}
$write username tty
{% endcodeblock %}

Now the question arises, how to know the tty of a particular user. The "who" command of UNIX comes in to play here. Just shoot "who" command on your terminal and see the magic. You will receive the list of all users logged on to that machine with their tty. Below is an example.

{% codeblock lang:bash %}
$who
root     :0           May  7 03:51
root     pts/1        May  7 03:51 (:0.0)
root     pts/2        May  7 03:51 (:0.0)
user5    pts/10       May  7 04:17 (dhcp-dwf1-4-10-176-162-74)
user8    pts/13       May  7 04:12 (dhcp-dwf1-4-10-176-162-180)
user9    pts/15       May  7 04:12 (10.176.146.212)
user7    pts/14       May  7 04:23 (dhcp-dwf1-4-10-176-162-118)
user12   pts/18       May  7 04:14 (dhcp-dwf1-4-10-176-162-119)
user11   pts/23       May  7 04:14 (dhcp-dwf1-4-10-176-162-31)
user1    pts/22       May  7 04:27 (dhcp-dwf1-4-10-176-162-125)
user15   pts/24       May  7 04:14 (dhcp-dwf1-4-10-176-162-65)
user6    pts/12       May  7 04:14 (dhcp-dwf1-4-10-176-162-120)
user2    pts/28       May  7 04:14 (dhcp-dwf1-4-10-176-162-60)
user7    pts/30       May  7 04:14 (dhcp-dwf1-4-10-176-162-118)
user4    pts/31       May  7 04:14 (dhcp-dwf1-4-10-176-162-72)
user10   pts/32       May  7 04:14 (dhcp-dwf1-4-10-176-162-68)
user14   pts/33       May  7 04:14 (dhcp-dwf1-4-10-176-162-27)
{% endcodeblock %}

If you want to send message to user8, issue following command at your terminal:

{% codeblock lang:bash %}
$write user8 pts/13
{% endcodeblock %}

Now write any message you want. To end the message, press Ctrl+D. Your message would be sent.
