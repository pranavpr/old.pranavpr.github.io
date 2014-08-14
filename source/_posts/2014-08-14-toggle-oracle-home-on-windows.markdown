---
layout: post
title: "Toggle ORACLE_HOME on Windows"
date: 2014-08-14 17:13:38 +0530
comments: true
categories: 
- Windows
- Oracle
---

I work in an environment where I constantly need to switch between 32-bit and 64-bit version of Oracle client. Manually editing the environment variable ORACLE_HOME becomes really annoying after some time. Hence I created a small batch file to toggle the ORACLE_HOME:

**toggle_oracle_home.bat**
{% codeblock lang:bat %}
@echo off
setlocal enableDelayedExpansion
if "!ORACLE_HOME:~-8!" == "dbhome_1" (
  set "ORCLHOME=C:\oracle\product\11.2.0\client_1"
) else (
  set "ORCLHOME=C:\oracle\product\11.2.0\dbhome_1"
)
setx ORACLE_HOME "!ORCLHOME!" /M
echo Oracle Home was set as : !ORCLHOME!
pause
{% endcodeblock %}

You can use this batch file to toggle another environment variable in Windows too by modifying lines 4, 6 and 8.
