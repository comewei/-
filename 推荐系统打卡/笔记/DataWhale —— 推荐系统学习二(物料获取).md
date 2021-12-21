### Scrapy 简介与安装

Scrapy 是一种快速的高级 web crawling 和 web scraping 框架，**用于对网站内容进行爬取，并从其页面提取结构化数据**。

Ubuntu下安装Scrapy，需要先安装依赖Linux依赖

```
sudo apt-get install python3 python3-dev python3-pip libxml2-dev libxslt1-dev zlib1g-dev libffi-dev libssl-dev
```

在新闻推荐系统虚拟conda环境中安装scrapy

```
pip install scrapy
```

#### Scrapy 项目结构

默认情况下，所有scrapy项目的项目结构都是相似的，在指定目录对应的命令行中输入如下命令，就会在当前目录创建一个scrapy项目

```shell
scrapy startproject myproject
```

运行之后出现![image-20211220144324603](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220144324603.png)

进入`myproject`，使用`tree` 查看项目结构:

```bash
tree
```

![image-20211220144425538](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220144425538.png)

- scrapy.cfg: 项目配置文件
- myproject/ : 项目python模块, 代码将从这里导入
- **myproject/ items.py: 项目items文件，**
- **myproject/ pipelines.py: 项目管道文件，将爬取的数据进行持久化存储**
- myproject/ settings.py: 项目配置文件，可以配置数据库等
- **myproject/ spiders/: 放置spider的目录，爬虫的具体逻辑就是在这里实现的（具体逻辑写在spider.py文件中）,可以使用命令行创建spider，也可以直接在这个文件夹中创建spider相关的py文件**
- myproject/ middlewares：中间件，请求和响应都将经过他，可以配置请求头、代理、cookie、会话维持等

#### spider

**spider是定义一个特定站点（或一组站点）如何被抓取的类，包括如何执行抓取（即跟踪链接）以及如何从页面中提取结构化数据（即抓取项）。换言之，spider是为特定站点（或者在某些情况下，一组站点）定义爬行和解析页面的自定义行为的地方。**

爬行器是自己定义的类，Scrapy使用它从一个网站(或一组网站)中抓取信息。它们必须继承 `Spider` 并定义要做出的初始请求，可选的是如何跟随页面中的链接，以及如何解析下载的页面内容以提取数据。

对于spider来说，抓取周期是这样的：

1. 首先生成对第一个URL进行爬网的初始请求，然后指定一个回调函数，该函数使用从这些请求下载的响应进行调用。要执行的第一个请求是通过调用 `start_requests()` 方法，该方法(默认情况下)生成 `Request` 中指定的URL的 `start_urls` 以及 `parse` 方法作为请求的回调函数。
2. 在回调函数中，解析响应(网页)并返回 [item objects](https://www.osgeo.cn/scrapy/topics/items.html#topics-items) ， `Request` 对象，或这些对象的可迭代。这些请求还将包含一个回调(可能相同)，然后由Scrapy下载，然后由指定的回调处理它们的响应。
3. 在回调函数中，解析页面内容，通常使用 [选择器](https://www.osgeo.cn/scrapy/topics/selectors.html#topics-selectors) （但您也可以使用beautifulsoup、lxml或任何您喜欢的机制）并使用解析的数据生成项。
4. 最后，从spider返回的项目通常被持久化到数据库（在某些 [Item Pipeline](https://www.osgeo.cn/scrapy/topics/item-pipeline.html#topics-item-pipeline) ）或者使用 [Feed 导出](https://www.osgeo.cn/scrapy/topics/feed-exports.html#topics-feed-exports) .

**下面是官网给出的Demo:**

```
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes" # 表示一个spider 它在一个项目中必须是唯一的，即不能为不同的spider设置相同的名称。
	
    # 必须返回请求的可迭代(您可以返回请求列表或编写生成器函数)，spider将从该请求开始爬行。后续请求将从这些初始请求中相继生成。
    def start_requests(self):
        urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/',
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse) # 注意，这里callback调用了下面定义的parse方法
	
    # 将被调用以处理为每个请求下载的响应的方法。Response参数是 TextResponse 它保存页面内容，并具有进一步有用的方法来处理它。
    def parse(self, response):
        # 下面是直接从response中获取内容，为了更方便的爬取内容，后面会介绍使用selenium来模拟人用浏览器，并且使用对应的方法来提取我们想要爬取的内容
        page = response.url.split("/")[-2]
        filename = f'quotes-{page}.html'
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log(f'Saved file {filename}')
```

#### Xpath

**XPath 是一门在 XML 文档中查找信息的语言，XPath 可用来在 XML 文档中对元素和属性进行遍历。在爬虫的时候使用xpath来选择我们想要爬取的内容是非常方便的**，这里就提一下xpath中需要掌握的内容，参考资料中的内容更加的详细（建议花一个小时看看）。

要了解xpath, 需要先了解一下HTML（是用来描述网页的一种语言）, 这个的细节就不详细展开

**划重点：**

1. **xpath路径表达式：**XPath 使用路径表达式来选取 XML 文档中的节点或者节点集。这些路径表达式和我们在常规的电脑文件系统中看到的表达式非常相似。节点是通过沿着路径 (path) 或者步 (steps) 来选取的。
2. **了解如何使用xpath语法选取我们想要的内容，所以需要熟悉xpath的基本语法**

### sinanews 项目实战

这里可以结合原文代码查看

##### 1.前置环境

1.1 在Ubuntu系统中安装好了MongoDB数据库，在Task01中有说到

1.2 terminal的conda环境中安装好了scrapy、pymongo包

##### 2. 项目逻辑

1. 每天定时从新浪新闻网站上爬取新闻数据存储到mongodb数据库中，并且需要监控每天爬取新闻的状态
2. 每天爬取新闻的时候只爬取当天日期的新闻，主要是为了防止相同的新闻重复爬取（当然这个也不能完全避免爬取重复的新闻，爬取新闻之后需要有一些单独的去重的逻辑）
3. 爬虫项目中实现三个核心文件，分别是sina.py（spider）,items.py（抽取数据的规范化及字段的定义），pipelines.py（数据写入数据库）

##### 3. 项目结构

3.1 首先创建scrapy项目，使用`tree`查看项目结构

```bash
scrapy startproject sinanews
cd sinanews
```

```
tree
```

![image-20211220144425538](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220144425538.png)

3.2 实现items.py逻辑，这里就不一一列举。[可以进入源码查看](https://github.com/datawhalechina/fun-rec/blob/master/codes/news_recsys/news_rec_server/materials/news_scrapy/sinanews/items.py)

3.3 **重点**—— 实现`spiders`中sina.py的逻辑

- 这里选择的是比较简单的界面，爬取会比较容易
- [源码入口](https://github.com/datawhalechina/fun-rec/blob/master/codes/news_recsys/news_rec_server/materials/news_scrapy/sinanews/spiders/sina.py)

3.4 爬取数据之后，实现数据持久化， pipeline.py

- 这里需要注意的就是实现SinanewsPipeline类的时候，里面很多方法都是固定的，不是随便写的，不同的方法又不同的功能，这个可以参考[scrapy中pipeline.py官方文档](https://docs.scrapy.org/en/latest/topics/item-pipeline.html)。官方文档里面对pipeline.py有很详细的说明。
- [源码入口](https://github.com/datawhalechina/fun-rec/blob/master/codes/news_recsys/news_rec_server/materials/news_scrapy/sinanews/pipelines.py)

3.5 配置文件 setting.py

- 这里是对整体spider配置
- 其他的文件middlewares.py项目中没有什么修改

3.6 monitor_news.py 监控脚步

- 对爬取的数据进行监控，如果数据爬取数量太少或者爬取不成功，会反馈报错

3.7 运行run_scrapy_sina.sh 脚步

- 这里比较坑，需要注意。每个人的项目环境不一致，设置python环境时需要特别注意



### 运行爬取项目，写入到MongoDB数据库

#### 1. 手动运行 run.py

```shell
python run.py --pages=30
```

#### 2. 在MongoDB端查看数据

```shell
mongo
```

进入MongoDB后查看数据大小

```Mongo
show dbs;
```

![image-20211220164145220](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220164145220.png)

这个是爬取前的数据(之前爬取的)

这个是爬取后的数据

![image-20211220170026874](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220170026874.png)

![image-20211220170045782](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220170045782.png)

> 参考

- [fun-rec GitHub开源项目](https://github.com/datawhalechina/fun-rec/blob/master/docs/%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E6%8E%A8%E8%8D%90%E7%B3%BB%E7%BB%9F%E5%AE%9E%E6%88%98/2.2%E6%96%B0%E9%97%BB%E6%8E%A8%E8%8D%90%E7%B3%BB%E7%BB%9F%E5%AE%9E%E6%88%98/docs/2.2.1.4%20scrapy%E5%9F%BA%E7%A1%80%E5%8F%8A%E6%96%B0%E9%97%BB%E7%88%AC%E5%8F%96%E5%AE%9E%E6%88%98.md)
- [MongoDB基础](https://github.com/datawhalechina/fun-rec/blob/master/docs/第二章 推荐系统实战/2.2新闻推荐系统实战/docs/MongoDB基础.md)
- [Scrapy框架新手入门教程](https://blog.csdn.net/sxf1061700625/article/details/106866547/)
- [scrapy中文文档](https://www.osgeo.cn/scrapy/index.html)
- [Xpath教程](https://www.w3school.com.cn/xpath/index.asp)

> 感悟

- Scrapy 的 官方文档写的真的很清晰，之前读的时候有点畏难心理





### 自动化构建用户及物料画像

#### 1. 前言

之前对`新浪微博`爬取过程得到的是离线自动化构建用户和物料画像。因为物料是来着爬虫获得的，所以还需要对爬取的数据进行处理，进而构造新闻的画像。

对于用户的画像而言，需要将新注册的用户添加到用户画像库中，对系统中产生了行为的用户，定期更新用户的画像。这里所说的行为包括`喜欢` `收藏`等，系统定期对用户画像更新。

下面从物料、用户两方面来描述系统如何自动化构建的



#### 2.物料侧画像的构建

 [sinanews 项目实战](#sinanews 项目实战) 提到了新物料的来源，但是这里物料需要注意的地方在于**每天凌晨的时候爬取前一天的新闻，可以爬到更多的物料，但是物料的时效性会延迟一天，新爬取的物料存储在MongoDB中**

##### 2.1 物料画像更新

- 新物料画像添加到物料库中（每日是先更新新物料，后更新旧物料。前后关系没有关联）
- 旧物料画像，通过用户交互记录进行更新

##### 2.2 新物料更新

爬取的数据存在`SinaNews`数据库中，对新物料进行简单处理，存储在的`NewsRecSys`数据库中的`FeatureProtrail`表中。在`DataGrid`中数据表对比可以看出

<img src=".\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220171949337.png" alt="image-20211220171949337" style="zoom:92.5%;" />

![image-20211220172054234](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220172054234.png)

处理逻辑：遍历“今天”爬取的所有文章，通过文章标题是否存在物料库中来去重。然后结合定义字段，存入画像物料池中



##### 2.3 旧物料更新

旧物料是指之前已经爬取的数据，曝光给了用户。用户行为会更新旧物料的特定字段，很明显，前端展现的`阅读`、`喜欢`、`收藏`会作为 本系统 推荐的重要指标。

![image-20211220174253115](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220174253115.png)

- 前端实时反馈
  - 系统提前把**新闻动态信息存储到了Redis**中，线上获取可以直接从redis中获取新闻数据，可以实时显示新闻的动态信息行为。如果用户对新闻产生了交互，信息会动态更新。比如你点击阅读，进入之后点击喜欢和返回，系统会动态显示新闻信息。这样可以试试获取新闻最新的动态画像信息
- Redis 每日更新
  - Redis 是 一个内存数据库，资源宝贵。有些价值不大的新闻(没有时效性)就需要删除。为了能够保存新闻历史的动态信息，系统还需要每天将redis中的动态新闻信息更新到mongodb存储的新闻画像库中。注意：**需要注意更新物料动态画像的时候一定得再redis数据清空之前**
- RedisProtrail 是 FeatureProtrail 的简化版



#### 3. 核心代码的阅读

news_protrait.py

- 主要是负责 
  - 新闻中用户行为更新到MongoDB数据库，修改到FeatureProtrail数据库。比如用户阅读、点击喜欢
  - Redis每日会清零，然后会从FeatureProtrail重新读取数据
  - 爬取数据简单处理输入到MongoDB数据库，新物料的导入
  - 这里插入一个示意图：![image-20211220215615791](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220215615791.png)

news_to_redis.py

Redis新闻信息的存储由两部分构成：静态信息和动态信息；比如静态信息：创建时间、标题、新闻内容，动态信息：点赞、喜欢等

- Redis 表格设计为4个表，在`dao_config.py`中有设定
  - 表一为推荐表
  - 表二为静态属性表
  - 表三为动态属性表
  - 表四为用户表

- ![image-20211220211019626](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220211019626.png)
- 这样存储的目的是为了线上实时修改物料动态信息的时候更加高效，减少不必要的传输
- 主要是负责：
  - ![image-20211220213919302](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220213919302.png) **备注一下，到回来看**—— 这里新闻比较少，简便使用
  - 每日对redis中的数据进行一次清洗，对不同数据类型处理后由MongoDB中的RedisProtrail转入Redis数据库

离线物料主要由以上两个函数构成，最后在`process_material.py` 中串起来。每天定时运行，物料侧逻辑就结束了

#### 4.用户侧画像的构建

##### 4.1 用户画像更新

- 新注册用户画像的更新
- 老用户画像的更新

##### 4.2 用户画像逻辑设定

由于我们系统中将所有注册过的用户都放到了一个表里面（新、老用户），所以每次更新画像的话只需要遍历一遍注册表中的所有用户。

用户表由以下字段构成：![image-20211220220836336](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211220220836336.png)

主要是用户的基本信息和用户历史信息相关的一些标签，对于用户的基本属性特征这个可以直接从注册表中获取，那么对于跟用户历史阅读相关的信息，需要统计用户历史的所有阅读、喜欢和收藏的新闻详细信息。为了得到跟用户历史兴趣相关的信息，我们需要对用户的历史阅读、喜欢和收藏这几个历史记录给存起来，其实这些信息都可以从日志信息中获取得到

__ __



点击文章阅读时，底部会有`喜欢`  、`收藏`按钮。这表明前端展示的结果是来源后端，后端需要提供用户历史点击**喜欢**及**收藏过的文章列表**。比较细节的是，这里我们使用Mysql数据库来存储，防止redis不够用。    这两个表不只是展示信息，还可以用来**分析用户画像**

![image-20211221105605517](.\DataWhale —— 推荐系统学习二(物料获取).assets\image-20211221105605517.png)

用户历史阅读的文章可以做用户画像，为了更好处理和理解，我们也维护了一份用户历史阅读过的所有文章的mysql表（维护表的核心逻辑就是每天跑一边用户日志，更新一下用户历史阅读的记录那么此时我们其实已经有了用户的阅读、点赞和收藏三个用户行为表了，接下来就直接可以通过这三个表来做具体的用户兴趣相关的画像

##### 4.3 用户画像代码分析

user_protrail.py

对MongoDB数据库用户画像进行更新，把当天注册的用户添加到用户画像池中。获取用户历史行为的统计特征["read","like","collection"]

process_user.py

用户画像总体逻辑：

用户注册——>更新到用户画像——>提取文章的阅读、点击、喜欢进行筛选——>加入到用户画像中

### 画像自动化构建

上面分别对用户侧和物料侧的画像构建进行了介绍，接下来就是要将上面所有的过程都自动化运行，并且设置好定时任务，其实**最核心的一点就是一定要在清除redis数据之前，完成用户和物料画像的构**建，下面是构建整个自动化的流程

物料更新——>用户画像更新——>redis数据更新——>离线数据更新

process_material.py——>process_user.py——>update_redis.py——>offline.py

##### Crontab定时任务(这里因为本人的run.py无法在阿里云服务器爬取，没有进行实践)：

```shell
0 0 * * * /home/recsys/news_rec_server/scheduler/crawl_news.sh >> /home/recsys/news_rec_server/logs/offline_material_process.log && 

/home/recsys/news_rec_server/scheduler/offline_material_and_user_process.sh >> /home/recsys/news_rec_server/logs/material_and_user_process.log && 

/home/recsys/news_rec_server/scheduler/run_offline.sh >> /home/recsys/news_rec_server/logs/offline_rec_list_to_redis.log
```

- 代码解读：
  - 前面的`0 0` 代表每天0点运行下面这一串脚本，上面命令的 `&&`表示先运行前面的，再运行后面的
  - 大致逻辑：
    1. 每天0点爬取新闻，作为新物料入库。这里注意爬取的是前天的新闻，前面有提及到
    2. 爬取之后，离散更新MongoDB中用户画像、物料画像及其线上要存储的redis画像
    3. 最后其实是离线推荐的流程，离线将用户的排序列表存到redis中，线上直接取就行了





> 代码要清晰、Util作为工具类使用、标注要通俗易懂

