---
layout: post
title: Basic Spider with Scrapy 
---

This post will explain how to create new scrapy project using command prompt and text editor such as notepad++. Note that it assumes you have already installed scrapy package on your machine, and I am using Scrapy version 1.1.2. See steps bewlow.

1. Open command prompt
2. Navigate to the directory where you want to create scrapy project – cd c:/users/yohan/documents/python27/projects
3. Input `scrapy startproject <project_name>` that generates the following structure
```
.
- scrapy.cfg
- <project_name>
    - __init__.py
    - items.py
    - pipelines.py
    - settings.py
    - spiders
        - __init__.py
```
4. After that, if you look at ./<project_name> directory then you will find items.py. This will contain the class to hold the scraped information. As an example, I would like to scrape job title and link from seek.com (job listing site) then the item class will look as below in items.py 

```python
from scrapy.item import Item, Field
class JobItem(Item):
    title = Field()
    link = Field()
```

{:start="5"}
5. Go ahead and create new file called test.py under /quick_scrapy/quick_scrapy/spiders folder. 
* I am going to create a SPIDER to crawl web pages and scrape info in this file. Spider will define initial url e.g. https://www.seek.com.au/jobs?keywords=software+engineer. It also defines how to follow links and pagination, and how extract and parse the field. Spider must define 3 attribute name, start url, parsing method. test.py has a spider class that includes the aforementioned attributes. This class has parsing method which takes the response of page call then parse info using xpath().

```python
from scrapy import Spider
from scrapy.selector import Selector
from quick_scrapy.items import JobItem

class JobSpider(Spider):
name = 'jobs'
allow_domains = ['seek.com.au']
start_urls = [
    'https://www.seek.com.au/jobs?keywords=software+engineer'
    ]

    def parse(self, reponse):
        titles = reponse.xpath('//article')
        items = []

        for each in titles:
            item = JobItem()
            item["title"] = each.xpath('@aria-label').extract()
            item["link"] = each.xpath('h1/a/@href').extract()
            print item['title'], item['link']

        return items
```

* When / is used at the beginning of path /a, it will define an absolute path to node ‘a’ from the root. When // is used at the beginning of path //a, it will define a path to node ‘a’ from from anywhere in xml response.
* When I run with – scrapy crawl jobs (identifier), Scrapy will make a call to the url and the response will be parsed
* You can easily find xpath of html element as below

![placeholder]({{ site.baseurl }}public/image/2017-01-29-scrapy-spider-1.png "Medium example image")

<div class="message">
    <strong>Application</strong>: Basic spider with scrapy is a good candidate to scrape target information under certain element path e.g. h1/a/@href without any exceptions or rules, and also retrieve information without following internal links i.e. page depth = 1 
</div>

See <a href="https://github.com/yohancheong/Scrapy-Spider">source code</a> 