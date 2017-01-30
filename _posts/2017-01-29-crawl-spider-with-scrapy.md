---
layout: post
title: Crawl Spider with Scrapy 
---

Basic spider has been discussed in previous post, which allows us to simply scrape information on pages specified on start_urls. However, there might be a case where internal links should be followed and certain urls should be filtered. 

This can be achieved with the help of CrawlSpider. Let's take a look at steps to develop it.

1. Open command prompt
2. Navigate to the directory where you want to create scrapy project e.g. cd c:/users/yohan/documents/python27/projects
3. Input `scrapy startproject <project_name>` that generates the following structure

- scrapy.cfg
- &lt;project_name&gt;
    - _init__.py
    - items.py
    - pipelines.py
    - settings.py
    - spiders
        - _init__.py

{:start="4"}
4. Now I need to create a spider class and it will be resided in new ./&lt;project_name&gt;/spiders/crawlspider.py
5. Let's say I am scraping links in wikipedia page then
* `name` is wikipedia
* `allowed_domains` is wikipedia.org
* `start_urls` is wikipedia page to start - https://en.wikipedia.org/wiki/Mathmetics

{:start="6"}
6. Main difference between BaseSpider and CrawlSpider is I can define a rule to follow internal links via `LinkExtractor`.
* `allow` that urls must match to be extracted (regular expression)
* `restrict_xpaths` is an xpath which defines regions inside the response where links should be extracted from 

