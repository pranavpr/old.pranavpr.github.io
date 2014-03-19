---
layout: post
title: "Switch between different JDK versions in Windows"
date: 2008-12-07
comments: true
categories:
 - Java
 - Tips
 - Windows
 - JDK
---

Being a Java developer, I always compile and test my code on different Java versions. But switching between them is a huge problem. So finally I found an easy method to do this. You have to create following batch files and place them in directory you open your command line in or in SYSTEM PATH. You can use you favorite text editor to create these files.

**jdk14.bat**
{% codeblock lang:bat %}
@echo off
echo Setting JAVA_HOME
set JAVA_HOME=C:\j2sdk1.4.2_12
echo setting PATH
set PATH=C:\j2sdk1.4.2_12\bin;%PATH%
echo Display java version
java -version
{% endcodeblock %}
**jdk15.bat**
{% codeblock lang:bat %}
@echo off
echo Setting JAVA_HOME
set JAVA_HOME=C:\Program Files\Java\jdk1.5.0_12
echo setting PATH
set PATH=C:\Program Files\Java\jdk1.5.0_12\bin;%PATH%
echo Display java version
java -version
{% endcodeblock %}
**jdk16.bat**
{% codeblock lang:bat %}
@echo off
echo Setting JAVA_HOME
set JAVA_HOME=C:\Program Files\Java\jdk1.6.0_11
echo setting PATH
set PATH=C:\Program Files\Java\jdk1.6.0_11\bin;%PATH%
echo Display java version
java -version
{% endcodeblock %}
Make sure you assign the appropriate JAVA_HOME value in batch files, according to your Java installation. Whenever you want to switch between Java versions, just run the respective batch file and you are done.

_Note- **JAVA_HOME** and the path to **java** must **always** refer to the exact same version of the JDK. If you mix them up, unpredictable things will happen!_
