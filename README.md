# dryao
# -*- coding: utf-8 -*-
import scrapy
from Toutiao_spider.items import ToutiaoSpiderItem
from scrapy.http.response.html import HtmlResponse
from scrapy.selector.unified import SelectorList
from Toutiao_spider.items import ToutiaoSpiderItem
import re

class ToutiaoSpider(scrapy.Spider):
    name = 'toutiao'
    allowed_domains = ['toutiao.com','']
    start_urls = ['https://www.toutiao.com/']
    base_domain = "https://www.toutiao.com"

#好用么？我觉得pycharm错。。据说现在微软对PYTHON支持不错啊~~~都可以吧，这个跟IDE没啥关系吧？

    def parse(self, response):
        a_list = response.xpath("//div//a[@class='link title']")
        for a in a_list:
            url =a.xpath('@href').extract_first()
            url ="https://www.toutiao.com"+url
            print(url)
            yield scrapy.Request(url,callback=self.parse_detail)

    def parse_detail(self,response):
        article_list =response.xpath("//div[@class='article-box']")
        c_list=response.xpath("//div[@class='article-content']//text()")
        for article in article_list:

            title =article.xpath("//h1/text()").get().strip()
            content =article.xpath("//div[@class='article-content']//text()").get()


            item={
                'title':title,
                'content':content,


            }



            yield item















