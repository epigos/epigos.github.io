---
layout: post
title: Create your own url shortner in python
description: Create your own url shortner in python using bitly API
keywords: url,shortner,python,bitly,bitlyapi
author: Philip Adzanoukpe
date: 2013-05-14 12:33:28
updated: 2013-05-14 12:33:28
tags: [python, url]
categories: post
---

URL shortening is a technique on the World Wide Web in which a Uniform Resource Locator (URL) may be made substantially shorter in length and still direct to the required page. This is achieved by using an HTTP Redirect on a domain name that is short, which links to the web page that has a long URL.

For example, the URL `http://en.wikipedia.org/wiki/URL_shortening` can be shortened to `http://bit.ly/urlwiki`, `http://is.gd/urlwiki` or `http://goo.gl/Gmzqv`.

This is especially convenient for messaging technologies such as Twitter and Identi.ca which severely limit the number of characters that may be used in a message. Short URLs allow otherwise long web addresses to be referred to in a tweet. In November 2009, the shortened links of the URL shortening service Bitly were accessed 2.1 billion times.

Bitly allows users to shorten, share, and track links (URLs). It's a way to save, share and discover links on the web. Bitly provides public API's which is intended to make it even easier for python programmers to use.

**Installing Bitly API Python**

The first step is to head over to dev.bitly.com where you will find the API documentation, best practices, code libraries, public data sets.

{% highlight bash %}
    pip install bitly_api
{% endhighlight %}

The bitly-api-python libray can be found in github.

**Shorten URL**

{% highlight python %}
def shorten_url(long_url):
    '''Return a shortened url using bitly api'''

    #define your bitly api credentials
    API_USER = "your bitly username"
    API_KEY = "your bitly api key"

    #import bitly python module
    import bitlyapi

    #create a new instance
    bitly = bitlyapi.BitLy(API_USER, API_KEY)

    #make request to bitly server
    response = bitly.shorten(longUrl=long_url)

    #get shortened url from the response
    short_url = response['url']

    return short_url
{% endhighlight %}

**Usage**
{% highlight python %}
    print shorten_url("http://en.wikipedia.org/wiki/URL_shortening")
{% endhighlight %}
