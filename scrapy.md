## scrapy 

http://scrapy-chs.readthedocs.io/zh_CN/latest/topics/shell.html

1. 开始一个scrapy项目：scrapy startproject project_name
					 scrapy genspider name(爬虫名字) domian(和这里面域名匹配的url才会被抓取)
> scrapy开启时会从start提取url并访问，此时调用start_requests(),大多数情况下需要重写start_requests()
> 然后把返回结果返回给parse（默认）
> 在运行爬虫时给爬虫传参：-a 参数名字=value，然后在__init__里面获取参数运用到爬虫中。
 
2. 分析你需要爬去的网站，并分析出需要的数据定义在items里面：name = scrapy.Field()

3. 在响应函数中对response对象进行数据提取：
> response.selector.xpath('//span/text()').extract(),简写response.xpath().extract(),返回一个SelectorList的实例。
> css选择器同理
> .re 正则匹配

4. Item Pipeline: 对收集到的item进行处理

> 每个Pipeline都会执行 process_item(self, item, spider)，要不返回一个Item，要不抛出Dropitem异常，被Drop的item不会被后面的Pipeline接收。

> 如果需要调用数据库，将数据库的初始化放在open_spider方法里面，并且在close_spider里面关闭连接。

> 启用Pineline组件需要添加到settings里面的ITEM_PIPELINES = {}。
> 在对Pineline进行初始话的时候使用

def __init__(self, mongo_uri, mongo_db):
        
self.mongo_uri = mongo_uri
       
 self.mongo_db = mongo_db

    @classmethod
    def from_crawler(cls, crawler):
        return cls(
            mongo_uri=crawler.settings.get('MONGO_URI'),
            mongo_db=crawler.settings.get('MONGO_DATABASE', 'items')
        )
