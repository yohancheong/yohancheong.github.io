---
layout: post
title: Basic Spider with Scrapy 
---

This post will explain how to create new scrapy project using command prompt and notepad++. Note that it assumes you have already installed scrapy library on your machine, and I am using Scrapy version 1.1.2

1. Open command prompt
2. Navigate to the directory where you want to create scrapy project – cd c:/users/yohan/documents/python27/projects
3. Start scrapy project by typing – scrapy startproject
4. After that, if you look at c:/users/yohan/documents/python27/projects/quick_scrapy/quick_scrapy directory then you will find items.py. This will contain a class to hold the scraped information. As an example, I would like to scrape job title and link from seek.com (job listing site) then the item class will look as below in item.py.
{% highlight python %}
from scrapy.item import Item, Field
class QuickScrapyItem(Item):
    title = Field()
    link = Field()
{% endhighlight %}

5. Go ahead and create new file called test.py under /quick_scrapy/quick_scrapy/spiders folder. I am going to create a SPIDER to crawl web pages and scrape info in this file. Spider will define initial url e.g. https://www.seek.com.au/jobs?keywords=software+engineer. It also defines how to follow links and pagination, and how extract and parse the field. Spider must define 3 attribute name, start url, parsing method. test.py has a spider class that includes the aforementioned attributes. This class has parsing method which takes the response of page call then parse info using xpath().
{% highlight python %}
from scrapy import Spider
from scrapy.selector import Selector
from quick_scrapy.items import QuickScrapyItem
 
class MySpider(Spider):
    name = 'jobs'
    allow_domains = ['seek.com.au']
    start_urls = ['https://www.seek.com.au/jobs?keywords=software+engineer']
 
    def parse(self, reponse):
        titles = reponse.xpath('//article')
        items = []
 
        for each in titles:
            item = QuickScrapyItem()
            item["title"] = each.xpath('@aria-label').extract()
            item["link"] = each.xpath('h1/a/@href').extract()
            print item['title'], item['link']
 
        return items
{% endhighlight %}

* When / is used at the beginning of path /a, it will define an absolute path to node ‘a’ from the root. When // is used at the beginning of path //a, it will define a path to node ‘a’ from from anywhere in xml response.
* When I run with – scrapy crawl jobs (identifier), Scrapy will make a call to the url and the response will be parsed
* You can easily find xpath of html element as below

![placeholder]({{ site.baseurl }}public/image/2017-01-29-scrapy-spider-1.png "Medium example image")