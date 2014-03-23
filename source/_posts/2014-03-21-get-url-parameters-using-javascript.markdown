---
layout: post
title: "Get URL parameters using JavaScript"
date: 2014-03-21 18:05:20 +0530
comments: true
categories: 
 - JavaScript
---

Suppose you want to get the parameters of an URL like below in JavaScript

{% codeblock lang:html %}
http://example.com?param1=value1&param2=value2
{% endcodeblock%}

Below JavaScript code might come handy in this case

{% codeblock lang:js %}
function getUrlVars() {
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m,key,value) {
        vars[key] = value;
    });
    return vars;
}

var myvar = getUrlVars();

for (var key in myvar) {
	alert(key+" = " + myvar[key]);
}
{% endcodeblock%}

It basically reads the current page url, perform some regular expression on the URL then saves the url parameters in an associative array, which we can easily access.