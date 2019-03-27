## ElasticSearch
- [Elasticsearch: 权威指南](https://elasticsearch.cn/book/elasticsearch_definitive_guide_2.x/index.html)

#### 为了搜索，你懂的

　　Elasticsearch是一个基于Apache Lucene(TM)的开源搜索引擎。无论在开源还是专有领域，Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。

　　但是，Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。

　　Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

　　不过，Elasticsearch不仅仅是Lucene和全文搜索，我们还能这样去描述它：
* 分布式的实时文件存储，每个字段都被索引并可被搜索
* 分布式的实时分析搜索引擎
* 可以扩展到上百台服务器，处理PB级结构化或非结构化数据

　　而且，所有的这些功能被集成到一个服务里面，你的应用可以通过简单的RESTful API、各种语言的客户端甚至命令行与之交互。

　　上手Elasticsearch非常容易。它提供了许多合理的缺省值，并对初学者隐藏了复杂的搜索引擎理论。它开箱即用（安装即可使用），只需很少的学习既可在生产环境中使用。

　　Elasticsearch在Apache 2 license下许可使用，可以免费下载、使用和修改。

　　随着你对Elasticsearch的理解加深，你可以根据不同的问题领域定制Elasticsearch的高级特性，这一切都是可配置的，并且配置非常灵活。

### 为什么 ElasticSearch 搜索效率高
- [时间序列数据库的秘密(1)-介绍](https://www.infoq.cn/article/database-timestamp-01)
- [时间序列数据库的秘密(2)-索引](https://www.infoq.cn/article/database-timestamp-02?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)  
- [时间序列数据库的秘密(3)-加载和分布式计算](https://www.infoq.cn/article/database-timestamp-03)  
上述系列文章的作者--->陶文，曾就职于腾讯 IEG 的蓝鲸产品中心，负责过告警平台的架构设计与实现。2006 年从 ThoughtWorks 开始职业生涯，在大型遗留系统的重构，持续交付能力建设，高可用分布式系统构建方面积累了丰富的经验。
- [为什么Elasticsearch/Lucene检索可以比MySQL快?](http://vlambda.com/wz_wvS2uI5VRn.html)
