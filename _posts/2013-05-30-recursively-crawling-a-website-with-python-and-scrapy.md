---
layout: post
title: Recursively crawling a website with Python and Scrapy
description: Recursively crawling a website with Python and Scrapy to get content of subpages in a listing page.
keywords: crawl,crawling,scrapy,website,recursive,python
author: Philip Adzanoukpe
date: 2013-05-30 12:33:28
updated: 2016-05-29 12:33:28
tags: [crawling, python, scrapy]
categories: post
---

[Scrapy][1] is a fast high-level screen scraping and web crawling framework, used to crawl websites and extract structured data from their pages. It can be used for a wide range of purposes, from data mining to monitoring and automated testing.

In this post I'm going to describe how you can use Scrapy to build recursive crawler for a website with listing page and pagination links. The crawler will scrap the listing pages, follow the listing URL to get the content and also follow the pagination links to get all the listings in the website.

If you are new to scrapy it is required to start by reading [Scrapy at a glance][2], then [download Scrapy][3] and follow the [Tutorial][4]. I also assume that you have Scrapy [installed][5] in your machine. If you know little bit of Python, you should be able to build your own web scraper within few minute.

Let's create a Scrapy project first using following command:

{% highlight bash %}
scrapy startproject demo
{% endhighlight %}

This will create a `demo` directory with the following contents:
{% highlight bash %}
demo/
    scrapy.cfg
    scrapy_demo/
        __init__.py
        items.py
        pipelines.py
        settings.py
        spiders/
            __init__.py
{% endhighlight %}

**Creating our Spider**

We'll create our spider using the crawl template for [Craigslist][6]
{% highlight python %}
scrapy genspider craigslist craigslist.org -t crawl
{% endhighlight %}

This will create a spider in the `spider` directory using this template
{% highlight python %}
# -*- coding: utf-8 -*-
import scrapy
from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import CrawlSpider, Rule

from $project_name.items import ${ProjectName}Item


class $classname(CrawlSpider):
    name = '$name'
    allowed_domains = ['$domain']
    start_urls = ['http://www.$domain/']

    rules = (
        Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),
    )

    def parse_item(self, response):
        i = ${ProjectName}Item()
        #i['domain_id'] = response.xpath('//input[@id="sid"]/@value').extract()
        #i['name'] = response.xpath('//div[@id="name"]').extract()
        #i['description'] = response.xpath('//div[@id="description"]').extract()
        return i
{% endhighlight %}

**Defining our Item**

Items are containers that will be loaded with the scraped data; they work like simple python dicts but provide additional protecting against populating undeclared fields, to prevent typos.

We begin by modeling the item that we will use to hold the sites data obtained from the website, as we want to capture the title, link and content of the sites, we define fields for each of these three attributes. To do that, we edit `items.py`, found in the `demo` directory. Our Item class looks like this:

{% highlight python %}
from scrapy.item import Item, Field


class CraigslistItem(Item):
    title = Field()
    link = Field()
    content = Field()
{% endhighlight %}

**Using item loaders**

[Item Loaders][7] provide a convenient mechanism for populating scraped Items.

In other words, Items provide the container of scraped data, while Item Loaders provide the mechanism for populating that container.

Item Loaders are designed to provide a flexible, efficient and easy mechanism for extending and overriding different field parsing rules, either by spider, or by source format (HTML, XML, etc) without becoming a nightmare to maintain

Declaring Item Loader for our spider inside the `items.py` file

{% highlight python %}
from scrapy.item import Item, Field
from scrapy.loader import ItemLoader
from scrapy.loader.processors import TakeFirst, Identity, Compose

class CraigslistItem(Item):
    title = Field()
    link = Field()
    content = Field()

class CraigslistItemLoader(ItemLoader):

    default_output_processor = TakeFirst()
    default_input_processor = Identity()

    # overide output processor for content
    content_out = Compose(TakeFirst(), lambda x: x.strip())

{% endhighlight %}

**Implementing the spider**

Our spider will define initial URL to download content from, how to follow pagination links and how to extract listing content in a page and creating items from the listings.

This spider would start crawling craigslist.org's home page, collecting category links, pagination links and item links, parsing the latter with the parse_item method. For each item response, some data will be extracted from the HTML using XPath, and an Item will be filled with it.

Our spider implementation will look like following:

{% highlight python %}
# -*- coding: utf-8 -*-
import scrapy
from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import CrawlSpider, Rule

from demo.items import CraigslistItem, CraigslistItemLoader


class CraigslistSpider(CrawlSpider):
    name = 'craigslist'
    allowed_domains = ['craigslist.org']
    start_urls = ['http://www.craigslist.org/']


    rules = (
        # Extract category links on homepage
        Rule(LinkExtractor(restrict_css='#center a')),
        # Extract pagination links on category page
        Rule(LinkExtractor(restrict_css='.paginator .next')),
        # Extract and follow item links on category page
        Rule(LinkExtractor(restrict_css='#sortable-results .rows .row'),
             callback='parse_item'),
    )

    def parse_item(self, response):
        l = CraigslistItemLoader(item=CraigslistItem(), response=response)
        # extract item link using response.url
        l.add_value('link', response.url)
        # extract item title using the xpath selector
        l.add_xpath('title', '//*[@id="titletextonly"]/text()')
        # extract item content using the xpath selector
        l.add_xpath('content', '//*[@id="postingbody"]/text()')
        return l.load_item()
{% endhighlight %}

**Running the scraper**

Now you can execute your scraper by running following command while in the root directory of your Scrapy project.

{% highlight bash %}
scrapy crawl craigslist
{% endhighlight %}

Scrapy allows you to save the scraped items into a JSON formatted file. All you have to do is add **-o filename.json -t json** option to previous crawl command. This will save the scraped items into a JSON file with the given name.

Here is a gist of [Mongodb pipeline][10]. You can find more information about [Scrapy pipeline][8] to save your scraped items into a database.

I will also recommend using the [scrapy cloud][9] to deploy and run your web spiders in the cloud


  [1]: http://scrapy.org/
  [2]: http://doc.scrapy.org/en/latest/intro/overview.html
  [3]: http://scrapy.org/download/
  [4]: http://doc.scrapy.org/en/latest/intro/tutorial.html
  [5]: http://doc.scrapy.org/en/latest/intro/install.html
  [6]: http://www.craigslist.org
  [7]: http://doc.scrapy.org/en/latest/topics/loaders.html
  [8]: http://doc.scrapy.org/en/latest/topics/item-pipeline.html
  [9]: http://scrapinghub.com/scrapy-cloud/
  [10]: https://gist.github.com/epigos/0185af79626ba6e00077


