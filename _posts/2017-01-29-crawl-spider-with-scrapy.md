---
layout: post
title: Crawl Spider with Scrapy 
---

Basic spider has been discussed in previous post, which allows us to simply scrape information on pages specified on start_urls. However, there might be a case where internal links should be followed and certain urls should be filtered. 

This can be achieved with the help of CrawlSpider. Let's take a look at steps to develop it.

1. Open command prompt
2. Navigate to the directory where you want to create scrapy project e.g. cd c:/users/yohan/documents/python27/projects
3. Input `scrapy startproject <project_name>` that generates the following structure
4. Now I need to create a spider class and it will be resided in new ./&lt;project_name&gt;/spiders/crawlspider.py
5. Let's say I am scraping links in wikipedia page then attributes for <ins>WikiSpider</ins> can be as below
* `name` is wikipedia
* `allowed_domains` is wikipedia.org
* `start_urls` is wikipedia page to start - https://en.wikipedia.org/wiki/Mathmetics

{:start="6"}
6. In contrast to BaseSpider, CrawlSpider allows us to define a rule that internal links is followed. See options for <ins>LinkExtractor</ins> below
* `allow` that urls must match to be extracted (regular expression)
* `restrict_xpaths` is an xpath which defines regions inside the response where links should be extracted from 

As I am targeting all links within main body of wiki page, the rule can be as below

```python
Rule(LinkExtractor(allow="https://en.wikipedia.org/wiki/", restrict_xpaths="//div[@class='mw-body']//a"), callback='parse_page', follow=False)
```

{:start="7"}
7. See the entire WikiSpider below

```python
from scrapy.contrib.spiders import CrawlSpider, Rule
from scrapy.contrib.linkextractors import LinkExtractor


class WikiSpider(CrawlSpider):

    name = 'wikipedia'
    allowed_domains = ['wikipedia.org']
    start_urls = ["https://en.wikipedia.org/wiki/Mathmetics",]

    rules = (
        Rule(LinkExtractor(
            allow="https://en.wikipedia.org/wiki/", 
            restrict_xpaths="//div[@class='mw-body']//a"), 
            callback='parse_page', 
            follow=False),
    )

    def parse_page(self, response):        
        print response.xpath('//h1[@class="firstHeading"]/text()').extract()
        return
```