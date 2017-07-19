# ChineseRegion
python 爬  统计用区划代码

基于https://github.com/Crawler-Home/ChineseRegion

自己做的5级信息全爬的爬虫

存储到mysql

运行建议
-----

因为公司要的id最少6位,我做了补0,可以自己更改

尽量在linux运行,稳定性好一些

region_spider.py 在index 中做分组  按省份运行多进程,要不会好慢

```python
 # 省
    def parse_start_url(self, response):
        type = 0
        links = response.css('.provincetable tr td a')
        self.title=len(links)
        for index in range(len(links)):
            # 这里建议用多进程要不或很慢  一共31个组  其中
            # if index!=1:
            #     continue
            link1 = links[index]
            url = link1.xpath('@href').extract()[0]
            name = link1.xpath('text()').extract()[0]
            urltemp = url[0:url.find('.')]
            insert(urltemp,name,type)
            url = response.urljoin(url)  # 补全
            logging.info(url)
            logging.info('*' * 30)
            yield Request(url, meta={"id": urltemp, 'name': name}, callback=self.parse_item)
```

运行结果
-----

我是在阿里云开10个进程爬的用时大约15个小时,当天运行第二天拿

一共31个省   我分组如下

0~3	99536
4~6	46128
7~9	45354
10~12	71679
13~15	158126
16~18	90638
19~21	34070
22~24	94800
25~27	49762
28~30	24274

一共714367条

