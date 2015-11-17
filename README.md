SeimiCrawler
==========
An agile,powerful,distributed crawler framework.

SeimiCrawler的目标是成为Java世界最好用最实用的爬虫框架。

# 简介 #

SeimiCrawler是一个敏捷的，支持分布式的爬虫开发框架，希望能在最大程度上降低新手开发一个可用性高且性能不差的爬虫系统的门槛，以及提升开发爬虫系统的开发效率。在SeimiCrawler的世界里，绝大多数人只需关心去写抓取的业务逻辑就够了，其余的Seimi帮你搞定。设计思想上SeimiCrawler受Python的爬虫框架Scrapy启发很大，同时融合了Java语言本身特点与Spring的特性，并希望在国内更方便且普遍的使用更有效率的XPath解析HTML，所以SeimiCrawler默认的HTML解析器是[JsoupXpath](http://jsoupxpath.wanghaomiao.cn)(独立扩展项目，非jsoup自带),默认解析提取HTML数据工作均使用XPath来完成（当然，数据处理亦可以自行选择其他解析器）。

# 原理示例 #
## 基本原理 ##
![SeimiCrawler原理图](http://77g8ty.com1.z0.glb.clouddn.com/v2_Seimi.png)

## 集群原理 ##
![SeimiCrawler集群原理图](http://77g8ty.com1.z0.glb.clouddn.com/v1_distributed.png)

# 快速开始 #

添加maven依赖(已经同步到中央maven库)：
```
<dependency>
    <groupId>cn.wanghaomiao</groupId>
    <artifactId>SeimiCrawler</artifactId>
    <version>0.1.1</version>
</dependency>
```

在包`crawlers`下添加爬虫规则，例如：
```
@Crawler(name = "basic")
public class Basic extends BaseSeimiCrawler {
    @Override
    public String[] startUrls() {
        return new String[]{"http://www.cnblogs.com/"};
    }
    @Override
    public void start(Response response) {
        JXDocument doc = response.document();
        try {
            List<Object> urls = doc.sel("//a[@class='titlelnk']/@href");
            logger.info("{}", urls.size());
            for (Object s:urls){
                push(new Request(s.toString(),"getTitle"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public void getTitle(Response response){
        JXDocument doc = response.document();
        try {
            logger.info("url:{} {}", response.getUrl(), doc.sel("//h1[@class='postTitle']/a/text()|//a[@id='cb_post_title_url']/text()"));
            //do something
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
然后随便某个包下添加启动Main函数，启动SeimiCrawler：
```
public class Boot {
    public static void main(String[] args){
        Seimi s = new Seimi();
        s.start("basic");
    }
}
```
以上便是一个最简单的爬虫系统开发流程。

# 更多文档 #

目前可以参考demo工程中的样例，基本包含了主要的特性用法。更为细致的文档移步[SeimiCrawler主页](http://seimi.wanghaomiao.cn)中进一步查看

# 正在TODO #
- 支持http服务API提交Request请求
- 添加Request的通用校验机制

# 项目源码 #
[Github](https://github.com/zhegexiaohuozi/SeimiCrawler)
> **BTW:**
> 如果您觉着这个项目不错，到github上`star`一下，我是不介意的 ^_^

# 联系我 #
- site: [www.wanghaomiao.cn](http://www.wanghaomiao.cn)
- email：`et.tw#163.com`