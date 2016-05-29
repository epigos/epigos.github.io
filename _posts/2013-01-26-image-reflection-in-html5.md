---
layout: post
title: Image Reflection in HTML5
description: create image reflection in web apps using HTML5 and CSS3
keywords: image,reflection,HTML5,CSS3
author: Philip Adzanoukpe
date: 2013-01-26 12:33:28
updated: 2013-01-26 12:33:28
tags: [HTML5]
categories: post
---

Image reflection in HTML5 using css3. Easy to create image reflections in your web apps using HTML5. This is the source code. The main action occurs in the css.

{% highlight css %}
img {float: right;

-webkit-box-reflect: below 4px

-webkit-gradient(linear,left top, left bottom, from(transparent),color-stop(.4, transparent),to(rgba(255,255,255,.5)));

-webkit-border-radius: 15px;}
{% endhighlight %}

{% highlight html %}
<h1>Image Reflections</h1>
<p><img src="images/img1.jpg"></p>
{% endhighlight %}
