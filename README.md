# alimama-webapi
无需阿里开放平台淘客权限，实现服务端全自动淘客抓取、佣金计划申请、优惠券生成、淘客链接创建等功能

# 目标

完全独立运行与服务器端的淘客数据生成工具，实现：

- 商品数据采集
- <b>采用非淘宝开放平台api</b>
- 查询是否加入淘宝客
- 申请淘客最高佣金计划
- 创建商品、店铺优惠券
- 创建淘客连接

# 思路

## 商品数据采集


> 商品数据采集主要是如下几个工作：
> 
> - 确定目标商品
> 
> 因为不是所有商品都参与了淘客计划，所以，需要找到正确的目标商品（当然后期通过阿里妈妈后台排除也是可以的）
>     
> - 抓取商品基础信息
> 
> 商品基础信息指的是商品详情，看下淘宝商品详情页里面的内容，这些就是需要的基础信息。所以，如果想要最准确的信息，只有找淘宝要。
> 
> - 维护商品基础信息
> 
> 商品基础信息是动态的，价格、库存、是否已经下架等等基础信息都不是一成不变的，所以，需要维护！（如果不在乎的话，也可以省略）


### 确定目标商品

商品数据采集主要有两种方式：

- 采集市面上比较大的淘客平台发布站点，如：实惠猪、淘客助手、大淘客、好单库等等网站
- 直接去阿里妈妈后台的超级搜索页面采集
- 其他方式...

<img width="350" src="https://github.com/poorevil/alimama-webapi/blob/master/readme_resource/dataoke.png"/>

<img width="350" src="https://github.com/poorevil/alimama-webapi/blob/master/readme_resource/shihuizhu.png"/>

<img width="350" src="https://github.com/poorevil/alimama-webapi/blob/master/readme_resource/alimama_search.png"/>

数据采集，主要用到的是爬虫，无外乎就是通过分析网页内容，编写爬虫脚本进行爬取。

根据我的知识体系，我选择了python的scrapy框架进行爬虫的编写，通过crontab做了一个定时任务，分别对几个更新比较频繁的网站进行实时爬取、入库。

### 抓取商品基础信息

最好的办法就是直接抓取淘宝的商品详情信息（这里不讨论用淘宝开放平台的接口，因为我没有权限，也懒得申请淘宝开放平台的接口）。

### 维护商品基础信息

既然已经抓取到商品信息了，最简单的方法就是定时任务了，自己搞个对比判断的逻辑，更新数据库把。


## 非淘宝开放平台api

12年还是13年那会儿，淘宝对淘客做了一次大面积的打击动作，很不幸，我也被误伤了，淘客账号被封。目前想要个开放平台的淘客权限好像很难。所以，干脆就不用开放平台的淘客权限吧。当然，我也知道好多人都在默默地这么搞着~

那么不用开放平台的接口怎么生成淘客连接呢？不是有阿里妈妈么？网页能生成啊，登录阿里妈妈，自己操作操作，人能操作，爬虫也能操作啊~

## 申请淘客高佣计划

一般的，直接用阿里妈妈后台的超级搜索进行生成淘客连接就已经完成工作了。但是佣金并不是最高的，因为，好多商家都创建了自己的高佣店铺计划（这里面又分自动审核通过和人工审核通过的），我们可以通过对比找到佣金最高的计划进行申请，如果是自动审核，那么就自动生效了~

<img width="350" src="https://github.com/poorevil/alimama-webapi/blob/master/readme_resource/yongjinjihua1.png"/>

<img width="350" src="https://github.com/poorevil/alimama-webapi/blob/master/readme_resource/yongjinjihua2.png"/>

当然，可能有更牛逼的方法，请知道的同学进行补充~

## 创建淘客连接

这里就是通过阿里妈妈后台直接创建淘客连接就ok了。此部分略~

## 创建商品、店铺优惠券

与创建淘客连接操作一样。此部分略~



# 实现


说了一大堆，这里才是重点哈~

大致思路就是，

- 爬虫爬取商品基础信息
- 模拟登陆阿里妈妈获取登录凭证
- 模拟提交阿里妈妈关键接口，实现最终目的


## 爬虫

根据个人喜好吧，我用的是python的scrapy框架，没别的顺手

## 模拟登陆

模拟登陆的难点在于阿里的登陆验证，如果同一个ip频繁登陆账号，会被要求滑动验证滑块~

这里算是有点窍门儿吧，主要是模拟鼠标事件，包括，鼠标移动、拖拽等操作。

一开始用的是casperjs + PhantomJS，也能模拟登陆，但是用的感觉不顺手。后来换成了puppeteer，感觉这个相当不错，一两百行就能搞定登陆操作，主要是方便调试！


## 生成淘客连接、优惠券连接、申请高佣计划等

这里没啥特别的，就是分析页面、编写爬虫。我用的还是scrapy~




## 最后

整体的解决方法差不多就这些吧，有啥问题可以在issue里提问或者加qq（13630574）联系我

以下是相关项目：

- <a href="#">爬虫项目</a>
- <a href="#">生成淘客连接项目</a>



