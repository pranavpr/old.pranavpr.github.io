---
layout: post
title: "Hello World!"
date: 2014-03-19 03:34:48 +0530
comments: true
categories: 
---

And again

{% codeblock lang:c %}
/***************************************************************************
 *
 * Copyright (C) 2014 Pranav Prakash, <pranav@pranavprakash.net>
 *
 * This software is distributed on an "AS IS" basis, WITHOUT WARRANTY OF ANY
 * KIND, either express or implied.
 *
 * Description: This program can login to a site and download multiple pages.
 * This program will download pages from starting to ending index.
 * e.g. Download URLs ending with integer like http://exampe.com/1 to
 * http://example.com/100
 *
 * Usage: downloader
 *
 * Parameters:
 *    URL: URL of webpage to download
 *    User ID: User Id to login
 *    Password: Password to login
 *    Start Index: Starting index
 *    Ending Index: Ending index
 *
 ***************************************************************************/
#include <stdio.h>
#define CURL_STATICLIB
#include <curl/curl.h>

// Define our struct for accepting LCs output
struct BufferStruct
{
    char *buffer;
    size_t size;
};

// This is the function we pass to LC, which writes the output to a BufferStruct
static size_t WriteMemoryCallback
(void *ptr, size_t size, size_t nmemb, void *data)
{
    size_t realsize = size * nmemb;

    struct BufferStruct *mem = (struct BufferStruct *) data;

    mem->buffer = realloc(mem->buffer, mem->size + realsize + 1);

    if ( mem->buffer )
    {
        memcpy( &( mem->buffer[ mem->size ] ), ptr, realsize );
        mem->size += realsize;
        mem->buffer[ mem->size ] = 0;
    }
    return realsize;
}

int main(void)
{
    int i;
    int intStartIdx, intEndIdx;
    char *strURL = malloc (256);
    char *strInitURL = malloc (256);
    char *strUserid = malloc (16);
    char *strPassword = malloc (16);
    char *strIdx = malloc (16);
    char *data = malloc (64);

    printf("%s", "Enter URL:");
    /* Get the URL, with size limit. */
    fgets (strInitURL, 256, stdin);
    /* Remove trailing newline, if there. */
    if ((strlen(strInitURL) > 0) && (strInitURL[strlen (strInitURL) - 1] == '\n'))
        strInitURL[strlen (strInitURL) - 1] = '\0';
    printf("%s", "Enter User ID:");
    /* Get the User ID, with size limit. */
    fgets (strUserid, 16, stdin);
    /* Remove trailing newline, if there. */
    if ((strlen(strUserid) > 0) && (strUserid[strlen (strUserid) - 1] == '\n'))
        strUserid[strlen (strUserid) - 1] = '\0';
    printf("%s", "Enter Password:");
    /* Get the Password, with size limit. */
    fgets (strPassword, 16, stdin);
    /* Remove trailing newline, if there. */
    if ((strlen(strPassword) > 0) && (strPassword[strlen (strPassword) - 1] == '\n'))
        strPassword[strlen (strPassword) - 1] = '\0';
    printf("%s", "Enter starting index:");
    scanf("%d", &intStartIdx);
    printf("%s", "Enter ending index:");
    scanf("%d", &intEndIdx);
    CURL *myHandle;
    CURLcode result; // We'll store the result of CURL's webpage retrieval, for simple error checking.
    struct BufferStruct output; // Create an instance of out BufferStruct to accept LCs output
    output.buffer = NULL;
    output.size = 0;
    for (i = intStartIdx; i <= intEndIdx; i++)
    {
        strURL[0] = '\0';
        strcat(strURL, strInitURL);
        sprintf(strIdx, "%d", i);
        strcat(strURL, strIdx);
        curl_global_init(CURL_GLOBAL_ALL);
        myHandle = curl_easy_init();
        // Set up a couple initial paramaters that we will not need to mofiy later.
        curl_easy_setopt(myHandle, CURLOPT_USERAGENT, "Mozilla/4.0");
        curl_easy_setopt(myHandle, CURLOPT_AUTOREFERER, 1);
        curl_easy_setopt(myHandle, CURLOPT_FOLLOWLOCATION, 1);
        curl_easy_setopt(myHandle, CURLOPT_COOKIEFILE, "");
        curl_easy_setopt(myHandle, CURLOPT_WRITEFUNCTION, WriteMemoryCallback); // Passing the function pointer to LC
        curl_easy_setopt(myHandle, CURLOPT_WRITEDATA, (void *)&output); // Passing our BufferStruct to LC

        // Visit the login page once to obtain a PHPSESSID cookie
        curl_easy_setopt(myHandle, CURLOPT_URL, strURL);
        curl_easy_perform(myHandle);
        // Now, can actually login. First we forge the HTTP referer field, or FP will deny the login
        curl_easy_setopt(myHandle, CURLOPT_REFERER, strURL);
        // Next we tell LibCurl what HTTP POST data to submit
        data[0] = '\0';
        strcat(data, "username=");
        strcat(data, strUserid);
        strcat(data, "&password=");
        strcat(data, strPassword);
        curl_easy_setopt(myHandle, CURLOPT_POSTFIELDS, data);
        curl_easy_perform(myHandle);
        strURL[0] = '\0';
        FILE *fp;
        fp = fopen("data.html", "a");
        if (!fp)
            return 1;
        fprintf(fp, output.buffer);
        fclose(fp);

        if (output.buffer)
        {
            free (output.buffer);
            output.buffer = 0;
            output.size = 0;
        }
        printf("%c", '=');
    }
    curl_easy_cleanup( myHandle );
    return 0;
}
{% endcodeblock %}

Some more 

Diff

{% codeblock lang:diff %}
@@ -38,12 +38,19 @@ destination: public
 plugins: plugins
 code_dir: downloads/code
 category_dir: blog/categories
-markdown: rdiscount
-rdiscount:
-  extensions:
-    - autolink
-    - footnotes
-    - smart
+# change markdown to kramdown from rdiscount
+#markdown: rdiscount
+#rdiscount:
+#  extensions:
+#    - autolink
+#    - footnotes
+#    - smart
+markdown: kramdown
+kramdown:
+  use_coderay: true
+  coderay:
+    coderay_line_numbers: table
+    coderay_css: class
 pygments: false # default python pygments have been replaced by pygments.rb
 
 paginate: 10          # Posts per page on the blog index
{% endcodeblock %}

Another test

{% codeblock lang:bat %}
@echo on
move "11Done\*" 
move "19Doone\*" 
move "41Done\*" 
move "29Done\*" 
move "21Done\*" 
move "50Done\*" 
move "2Done\*" 
move "60Done\*" 
move "13Done\*" 
move "32Done\*" 
move "16Done\*" 
move "58Done\*" 
move "8Done\*" 
move "23Done\*" 
move "43Done\*" 
move "17Done\*" 
move "35Done\*" 
move "37Done\*" 
move "53Done\*" 
move "25Done\*" 
move "20Done\*" 
move "5Done\*" 
move "9Done\*" 
move "57Done\*" 
move "55Done\*" 
move "49Done\*" 
move "7Done\*" 
move "12Done\*" 
move "1Done\*" 
move "59Donee\*" 
move "45Done\*" 
move "36Done\*" 
move "48Done\*" 
move "6Done\*" 
move "22Done\*" 
move "15Done\*" 
move "42Done\*" 
move "18Done\*" 
move "38Done\*" 
move "51Done\*" 
move "28Done\*" 
move "54Done\*" 
move "3Done\*" 
move "39Done\*" 
move "56Done\*" 
move "27Done\*" 
move "46Done\*" 
move "10Done\*" 
move "40Donee\*" 
move "44Done\*" 
move "24Done\*" 
move "61Done\*" 
move "34Done\*" 
move "47Done\*" 
move "31Done\*" 
move "30Done\*" 
move "4Done\*" 
move "52Done\*" 
move "26Done\*" 
move "14Done\*" 
move "20 Done v2\*" 
move "33Done\*" 
{% endcodeblock %}

Gist

{% gist 4321346 %}