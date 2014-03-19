---
layout: post
title: "How to create a custom IE8 Accelerator"
date: 2009-06-29
comments: true
categories:
 - IE8
 - Tips
 - Windows
 - Accelerator
---

Microsoft has introduced a new feature in Internet Explorer 8 called "Accelerators". These accelerators help you quickly perform your everyday browsing tasks without navigating to other websites to get things done. Simply highlight text from any webpage, and then click on the blue Accelerator icon that appears above your selection to perform the respective task.
<!--more-->
<div style="TEXT-ALIGN: center;"><img src="http://2.bp.blogspot.com/__GfZLYkxICU/Skj6BrTx--I/AAAAAAAALGs/ctho7T6mIYE/s400/d_screenshot_accelerator.jpg" /></div>

## How to create an "Accelerator"?

Well it's a pretty simple two step process:

1. Create XML file
2. Create accelerator installation link

Let us create a simple accelerator which queries IMDB for movie titles and displays the result.

## IMDB Accelerator

On closer observation, we find that URL for search page of IMDB is of form

http://www.imdb.com/find?q=SearchString

where "SearchString" is the string you are querying for. With this information, lets start making the accelerator.

## Step 1: Create XML File

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<openServiceDescription xmlns="http://www.microsoft.com/schemas/openservicedescription/1.0">
<homepageUrl>http://www.imdb.com </homepageUrl>
<display>
 <name>Search IMDB </name>
 <icon>http://www.imdb.com/favicon.ico </icon>
</display>
<activity category="Search">
<activityAction context="selection">
 <execute action="http://www.imdb.com/find">
  <parameter name="q" value="{selection}" type="text" />
 </execute>
</activityAction>
</activity>
</openServiceDescription>
{% endcodeblock %}

This XML file is called the service description file and it contains specific description about the service provider and type of service it provides. The {selection} value on Line 11 refers to the text that you may highlight on a web page before performing the search. Save this file as search.xml.

## Step 2: Create accelerator installation link

To let users use our accelerator, we will create a install button. Users can install the accelerator using this link. Here the code for link:

{% codeblock lang:html %}
<a href="javascript:window.external.AddService('http://pranavprakash.net/blog/search.xml')">Install Accelerator</a>
{% endcodeblock %}

That's it, you are done. Install the accelerator and start searching.

<div style="TEXT-ALIGN: center;"><img src="http://2.bp.blogspot.com/__GfZLYkxICU/Skjr2D4faUI/AAAAAAAALGk/ys4EzTCU2fk/s400/imdb_accelerator.jpg" /></div>

You will find your accelerator in "All Accelerators" context menu item.