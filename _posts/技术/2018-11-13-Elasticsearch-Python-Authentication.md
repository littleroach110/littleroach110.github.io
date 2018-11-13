# 1. 目的

默认基础版的Elasticsearch没有开启用户名和密码的认证功能，这种情况下，所有的人都可以对Elasticsearch数据库进行增删改查操作，如果把Elasticsearch当作唯一的存储数据库，则会带来数据安全隐患，因此需要解决和开启Elasticsearch的用户认证功能。同时，支持Python对Elasticsearch进行增删改查的数据库操作。

> PS：所有的操作在单机完成，如果是在集群上进行操作，增加集群的配置即可。

# 2. 安装过程

### 2.1 配置X-Pack并破解

具体内容，请参考网页文档：[Elasticsearch+Kibana 6.4.2 yum安装配置破解全过程](https://blog.csdn.net/qq_36731677/article/details/83090036)

> 注意，在编译java程序是，注意文件路径信息

### 2.2 Python调用操作

配置完成以后，不需要对```elasticsearch.yml```文件进行任何操作，使用命令 ```curl 'http://localhost:9200' -u username:password```就可以查看到Elasticsearch的基本信息。

但是如果要使用Python对Elasticsearch进行操作时，在[官方文档](https://elasticsearch-py.readthedocs.io/en/master/)中，介绍的是开启SSL时调用的Python示例代码，但是Elasticsearch配置SSL的过程，满路崎岖，最终放弃使用SSL，而选择普通的HTTP进行用户认证和API接口调用。示例代码如下：

```
# -*- encoding:utf-8 -*-
from elasticsearch import Elasticsearch

dsl = {
    'query': {
        'match': {
            'tag': 'ddos'
        }
    }
}

es = Elasticsearch(['localhost:9200'], http_auth="username:password")
result = es.search(index='_all', body=dsl)
print(result)
```

其中，dsl为elasticseach查询语句，即可实现Python对Elasticsearch进行调用，同时使用了用户名和密码进行身份认证。