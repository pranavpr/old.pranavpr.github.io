---
layout: post
title: "How to find out CPU utilization in Linux?"
date: 2009-02-19
comments: false
categories:
 - Tips
 - Linux
 - CPU Utilization
---

Everyone knows that CPU utiization in windows can be found out from Windows Task Manager. But what about Linux? Well Linux has also got set of utilities to monitor CPU utilization. With these commands you can find out total CPU utilization, individual CPU utilization (for SMP machines), your system's average CPU utilization since the last reboot, determine which process is eating up your CPU(s) etc.

## Good old "top" command

The top command provides dynamic view of CPU utilization. It displays system information as well as list of tasks currently managed by kernel. Is also displays uptime, average load,  physcal and swap memory utilization. Syntax of top command is as follows:

{% codeblock lang:bash %}
$ top
{% endcodeblock %}

<div style="TEXT-ALIGN: center"><img src="http://1.bp.blogspot.com/__GfZLYkxICU/SZ1EdKuhqvI/AAAAAAAAKaQ/IOyFqTaYkPU/s400/top.jpeg" /></div>

To quit "top", you have to press Q key of your keyboard.

## Using "mpstat" command

To use this command, you have to install a package called **sysstat**. For Ubuntu or Debian systems, you can install this package using apt-get.

{% codeblock lang:bash %}
$ apt-get install sysstat
{% endcodeblock %}

To get CPU utilization information, type in following command:

{% codeblock lang:bash %}
$ mpstat
{% endcodeblock %}
<div style="TEXT-ALIGN: center"><img src="http://3.bp.blogspot.com/__GfZLYkxICU/SZ1HYqBN79I/AAAAAAAAKaY/ao4jzWcMSus/s400/mpstat.jpeg" /></div>

To monitor individual processor performance, issue following command:

{% codeblock lang:bash %}
$ mpstat -P ALL
{% endcodeblock %}

<div style="TEXT-ALIGN: center"><img src="http://1.bp.blogspot.com/__GfZLYkxICU/SZ1JWeK086I/AAAAAAAAKag/R4CJFb-PlnA/s400/mpstat+all.jpeg" /></div>

## The "sar" command

To display CPU utilization using "sar", use following command:

{% codeblock lang:bash %}
$ sar -u 2 5
{% endcodeblock %}

This command will display CPU utilization 2 seconds apart, 5 times as shown below.

<div style="TEXT-ALIGN: center"><img src="http://4.bp.blogspot.com/__GfZLYkxICU/SZ1K8P-YVzI/AAAAAAAAKao/f2vTLV8Zkjk/s400/sar.jpeg" /></div>

##The "iostat" command

The iostat command reports Central Processing Unit (CPU) statistics and input/output statistics for devices and partitions. It can be use to find out your system's average CPU utilization since the last reboot.

{% codeblock lang:bash %}
$ iostat
{% endcodeblock %}

<div style="TEXT-ALIGN: center"><img src="http://4.bp.blogspot.com/__GfZLYkxICU/SZ1LmKj8Z3I/AAAAAAAAKaw/EeBQkTTH7v8/s400/iostat.jpeg" /></div>

## GUI Tools

KDE desktop environment has a system monitor utility which shows CPU utilization as well as many more information. You can also kill a process using this utility as shown below:

<div style="TEXT-ALIGN: center"><img src="http://3.bp.blogspot.com/__GfZLYkxICU/SZ1M9w041ZI/AAAAAAAAKa4/G-bjtEnPAPA/s400/sysmonitor.jpeg" /></div>

It also gives CPU load information, physical and swap memory usage data in graphical format as shown below:

<div style="TEXT-ALIGN: center"><img src="http://1.bp.blogspot.com/__GfZLYkxICU/SZ1NZMUJ8DI/AAAAAAAAKbA/Uza7zAQCz8E/s400/sysmonitor+more.jpeg" /></div>

For learning more about above commands and their options, you can go through their man pages.
