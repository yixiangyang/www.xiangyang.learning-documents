es学习整理

一、安装

在安装Elasticsearch的时候需要先创建个es专用的普通用户，es不允许root用户启动

坑1:安装最新的es7.4需要jdk11以上的版本

坑2:在安装Elasticsearch的时候,报错说cms 垃圾收集器在 jdk9 就开始被标注为启用所以这里需要将垃圾回收器从

 -XX:+UseConcMarkSweepGC 替换为-XX:+UseG1GC

坑3:在安装Elasticsearch的时候,由于我服务器内存比较小，报错（默认是1g）,我这里将他改为

-Xms100m
-Xmx100m

坑4：es用户的最大线程数

坑5：es用户的线程打开最大文件数

elasticsearch-head 的安装中

npm install -g  打包elasticsearch-head

下载grunt npm instal -g grunt-cli

报grunt 在本地没有的时候输入此命令 npm install grunt --save-dev



npm install phantomjs-prebuilt@2.1.16   --ignore-scripts

二、开始使用插入数据

1、插入一个客户的数据

```console
PUT /customer/_doc/1
{
  "name": "John Doe"
}
```

2、获取客户的详情数据

```console
GET /customer/_doc/1
```

3、批量插入账户的数据

```sh
curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_bulk?pretty&refresh" --data-binary "@accounts.json"
```

4、开始搜索--按照账号进行排序搜索账号的所有文档

```console
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```

该响应还提供有关搜索请求的以下信息：

- `took` – Elasticsearch运行查询所需的时间（以毫秒为单位）
- `timed_out` –搜索请求是否超时
- `_shards` –搜索了多少个分片，以及成功，失败或跳过了多少个分片。
- `max_score` –找到的最相关文件的分数
- `hits.total.value` -找到了多少个匹配的文档
- `hits.sort` -文档的排序位置（不按相关性得分排序时）
- `hits._score`-文档的相关性得分（使用时不适用`match_all`）

每个搜索请求都是独立的：Elasticsearch在请求中不维护任何状态信息。如果需要分页，在请求中指定`from`和`size`参数。

5、使用分页查询第9条到第19的数据

```console
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ],
  "from": 10,
  "size": 10
}
```

6、使用match搜索`address`中包含mill 或lane的数据

```console
GET /bank/_search
{
  "query": { "match": { "address": "mill lane" } }
}
```

7、执行词组搜索而不是单个单词搜索使用`match_phrase`代替`mathch`

```console
GET /bank/_search
{
  "query": { "match_phrase": { "address": "mill lane" } }
}
```

8、构造更复杂的查询使用bool来组合多个查询条件可以根据`must`必须匹配对应sql的and和must_not必须不匹配对应sql的!(非)查询年龄为40 state不为ID的所有人，should 是包含对应sql的or

```console
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}
```

9、使用范围过滤器查询余额在20000到30000之间的账户

`range`范围

`gte`最小

`lte`最大

```console
GET /bank/_search
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}
```

三、使用Elasticsearch做一些数据分析和汇总

1、使用`terms`汇总，将bank索引中的所有账户按照状态分组，并按照降序返回账户数量做多的十个州

```console
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      }
    }
  }
}
```

返回

`buckets`响应中的值是state字段的key ，doc_count是该分组在每个state的数量因为size=0所以该响应只包含聚合的结果

```js
{
  "took": 29,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped" : 0,
    "failed": 0
  },
  "hits" : {
     "total" : {
        "value": 1000,
        "relation": "eq"
     },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_state" : {
      "doc_count_error_upper_bound": 20,
      "sum_other_doc_count": 770,
      "buckets" : [ {
        "key" : "ID",
        "doc_count" : 27
      }, {
        "key" : "TX",
        "doc_count" : 27
      }, {
        "key" : "AL",
        "doc_count" : 25
      }, {
        "key" : "MD",
        "doc_count" : 25
      }, {
        "key" : "TN",
        "doc_count" : 23
      }, {
        "key" : "MA",
        "doc_count" : 21
      }, {
        "key" : "NC",
        "doc_count" : 21
      }, {
        "key" : "ND",
        "doc_count" : 21
      }, {
        "key" : "ME",
        "doc_count" : 20
      }, {
        "key" : "MO",
        "doc_count" : 20
      } ]
    }
  }
}
```

2、`aggs`聚合的模板

当query和aggs一起存在时，会先执行query的主查询，主查询query执行完后会搜出一批结果，而这些结果才会被拿去aggs拿去做聚合

按照状态分组，并且计算出账户的平均金额

```console
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```

3、通过`terms`聚合内的顺序来使用嵌套聚合的结果进行排序，而不是按照计数对结果进行排序

```console
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword",
        "order": {
          "average_balance": "desc"
        }
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```

三、Aggregation

(一)Avg Aggreation平均聚合

1、