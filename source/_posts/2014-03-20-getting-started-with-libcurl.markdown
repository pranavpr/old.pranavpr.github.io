---
layout: post
title: "Getting started with LibCurl"
date: 2014-03-20 21:36:51 +0530
comments: true
categories: 
 - C
 - Open Source
 - Windows
---

[LibCurl](http://curl.haxx.se/libcurl/) is an open-source client-side URL transfer utility supporting multiple protocols. LibCurl is supported by almost every conceivable common platform, making it one of the most versatile libraries of its kind.

In this post, I will show you how to use LibCurl in C to make simple HTTP requests with its built-in cookie processor to programmatically log in to websites and download web pages.

## Setup / Installation

Download the LibCurl for your platform form [here](http://curl.haxx.se/download.html). I would be using it on Windows with MinGW compiler and hence downloaded [Win32 - Generic](http://curl.haxx.se/gknw.net/7.34.0/dist-w32/curl-7.34.0-devel-mingw32.zip) build. I extracted the zip file and copied the contents of "include" and "lib" folder to MinGW's "include" and "lib" folders respectively. That's it, you are ready to write your C program.
<!--more-->
## Retrieving a Webpage

The program which I am going to write will POST the login data to a login page, retrieve the cookie and use it for downloading webpage.

First we need to include LibCurl's header file

{% codeblock lang:c %}
#include <curl/curl.h>
{% endcodeblock %}

Now we need to initialize the LibCurl globally as below

{% codeblock lang:c %}
curl_global_init(CURL_GLOBAL_ALL);
{% endcodeblock %}

This will initialize the LibCurl's module. Now we need to create a handle to make HTTP requests.

{% codeblock lang:c %}
CURL *myHandle;
CURLcode result; // We'll store the result of CURL's webpage retrieval, for simple error checking.
myHandle = curl_easy_init();
{% endcodeblock %}

Now we will define our user agent as some websites reject HTTP requests without a proper user agent. We'll also specify for LibCurl to follow redirects, as many login pages redirect users to a home screen. Remember, we only need to do this once. LibCurl will keep these settings in effect unless we change them.

{% codeblock lang:c %}
curl_easy_setopt(curl, CURLOPT_USERAGENT, "Mozilla/4.0");
curl_easy_setopt(curl, CURLOPT_AUTOREFERER, 1 );
curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1 );
{% endcodeblock %}

Next, we'll enable LibCurl's automatic processing of cookies.

{% codeblock lang:c %}
curl_easy_setopt(curl, CURLOPT_COOKIEFILE, "");
{% endcodeblock %}

Now we'll tell LibCurl which URL to fetch.

{% codeblock lang:c %}
curl_easy_setopt(myHandle, CURLOPT_URL, "http://www.example.com");
{% endcodeblock %}

Let's visit the login page once to obtain a PHPSESSID cookie.
{% codeblock lang:c %}
curl_easy_perform(myHandle);
{% endcodeblock %}

Next forge the HTTP referer field, or website will deny the login.
{% codeblock lang:c %}
curl_easy_setopt(myHandle, CURLOPT_REFERER, http://www.example.com);
{% endcodeblock %}

Now we'll prepare the data to post to login form.

{% codeblock lang:c %}
char *data="username=your_username_here&password=your_password_here";
curl_easy_setopt(curl, CURLOPT_POSTFIELDS, data);
{% endcodeblock %}

You may have to change data string according to the structure of login page. Next we'll make the second HTTP POST request to obtain the real page.

{% codeblock lang:c %}
curl_easy_perform(myHandle);
{% endcodeblock %}

And lastly we'll cleanup.

{% codeblock lang:c %}
curl_easy_cleanup(myHandle);
{% endcodeblock %}

Below is the complete code.

{% codeblock lang:c %}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define CURL_STATICLIB
#include <curl/curl.h>

int main()
{
    curl_global_init(CURL_GLOBAL_ALL);
    CURL *myHandle = curl_easy_init();

    // Set up a couple initial paramaters that we will not need to mofiy later.
    curl_easy_setopt(myHandle, CURLOPT_USERAGENT, "Mozilla/4.0");
    curl_easy_setopt(myHandle, CURLOPT_AUTOREFERER, 1);
    curl_easy_setopt(myHandle, CURLOPT_FOLLOWLOCATION, 1);
    curl_easy_setopt(myHandle, CURLOPT_COOKIEFILE, "");

    // Visit the login page once to obtain a PHPSESSID cookie
    curl_easy_setopt(myHandle, CURLOPT_URL, "http://www.example.com/login/");
    curl_easy_perform(myHandle);

    // Now, can actually login. First we forge the HTTP referer field, or website will deny the login
    curl_easy_setopt(myHandle, CURLOPT_REFERER, "http://www.example.com/login/");

    // Next we tell LibCurl what HTTP POST data to submit
    char *data = "username=your_username_here&password=your_password_here";
    curl_easy_setopt(myHandle, CURLOPT_POSTFIELDS, data);
    curl_easy_perform(myHandle);
    
    // Cleanup
    curl_easy_cleanup(myHandle);

    return 0;
}
{% endcodeblock %}

Next we'll compile the code.

{% codeblock lang:bat %}
gcc downloader.c -o downloader.exe -lcurl -lwsock32 -lidn -lwldap32 -lssh2 -lrtmp -lcrypto -lz -lws2_32 -lwinmm -lssl
{% endcodeblock %}

The executable file thus generated can be executed to fetch the webpage.

## Epilogue

LibCurl is used in numerous real-world applications. Refer [this](http://curl.haxx.se/libcurl/using/apps.html) page for a list of such applications. You can customize the code above to make applications suited to your need.