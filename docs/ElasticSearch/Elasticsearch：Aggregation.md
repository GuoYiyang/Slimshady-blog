# Elasticsearch:Aggregation

>   参考文档：https://www.elastic.co/guide/en/elasticsearch/guide/current/aggregations.html

## 什么是Aggregation(聚合函数)

ES提供的聚合功能可以用来进行简单的数据分析，ES中的聚合上可以分为下面两类：

1.  **metric**（度量）聚合：度量类型聚合主要针对的number类型的数据，需要ES做比较多的计算工作
2.  **bucketing**（桶）聚合：划分不同的“桶”，将数据分配到不同的“桶”里。非常类似sql中的group语句的含义。

>   es中的Bucket类似于SQL中的Group By，根据条件对元素进行分组；而Metric则和SQL中的SUM，MAX作用相似。

#### 桶(Buckets)

一个桶就是满足特定条件的一个文档集合：

*   一名员工要么属于男性桶，或者女性桶。
*   城市Albany属于New York州这个桶。
*   日期2014-10-28属于十月份这个桶。

随着聚合被执行，每份文档中的值会被计算来决定它们是否匹配了桶的条件。如果匹配成功，那么该文档会被置入该桶中，同时聚合会继续执行。

桶也能够**嵌套**在其它桶中，能让你完成层次或者条件划分这些需求。比如，Cincinnati可以被放置在Ohio州这个桶中，而整个Ohio州则能够被放置在美国这个桶中。

ES中有很多类型的桶，可以将文档通过多种方式进行划分(按小时，按最流行的词条，按年龄区间，按地理位置，以及更多)。但是从根本上，它们都根据相同的原理运作：**按照条件对文档进行划分**。

#### 指标(Metrics)

桶能够让我们对文档进行有意义的划分，但是最终我们还是需要对每个桶中的文档进行某种指标计算。分桶是达到最终目的的手段：提供了对文档进行划分的方法，从而能够计算需要的指标。

多数指标仅仅是简单的数学运算(比如，min，mean，max以及sum)，它们使用文档中的值进行计算。在实际应用中，指标能够计算例如平均薪资，最高出售价格，或者百分之95的查询延迟。

#### 结合起来

一个聚合就是一些桶和指标的组合。一个聚合可以只有一个桶，或者一个指标，或者每样一个。在桶中甚至可以有多个嵌套的桶。比如，我们可以将文档按照其所属国家进行分桶，然后对每个桶计算其平均薪资(一个指标)。

因为桶是可以嵌套的，我们能够实现一个更加复杂的聚合操作：

1.  将文档按照国家进行分桶。
2.  然后将每个国家的桶再按照性别分桶。
3.  然后将每个性别的桶按照年龄区间进行分桶。
4.  最后，为每个年龄区间计算平均薪资。

此时，就能够得到每个<国家，性别，年龄>组合的平均薪资信息了。它可以通过一个请求，一次数据遍历来完成。

## 示例

#### 简单查询

示例：查询各个年龄的人数分布：

查询语句如下：

```json
GET /bank/_search
{
  "query": {"match_all": {} }, 
  "aggs": {
    "city": {
      "terms": {
        "field": "age",
        "size": 10
      }
    }
  }
}
```

解释：

*   聚合语句写在第一层aggs参数之下（aggs也可以写成aggregations）
*   colors是该aggregations的名称。可以随意命名
*   terms是bucket的一种类型。

数据都是存放在一个桶里面，查询语句就是为了把数据从大桶里面捞出来然后放到小桶里面进行其他的操作。在该查询语句中，terms指定的field是age，就是为每个年龄建立sub bucket（小桶），把那些数据放入几个小桶中进行重新归类。

查询结果如下：

```json
  "aggregations" : {
    "city" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 463,
      "buckets" : [
        {
          "key" : 31,
          "doc_count" : 61
        },
        {
          "key" : 39,
          "doc_count" : 60
        },
        {
          "key" : 26,
          "doc_count" : 59
        },
        {
          "key" : 32,
          "doc_count" : 52
        },
        {
          "key" : 35,
          "doc_count" : 52
        },
        {
          "key" : 36,
          "doc_count" : 52
        },
        {
          "key" : 22,
          "doc_count" : 51
        },
        {
          "key" : 28,
          "doc_count" : 50
        },
        {
          "key" : 33,
          "doc_count" : 50
        },
        {
          "key" : 34,
          "doc_count" : 49
        }
      ]
    }
  }
```

#### 复杂查询

简单的buckets及计算每个bucket中的document number是一个基础分类，但是在应用中我们往往需要更复杂的统计。

示例：查询各个年龄的平均收入

查询语句如下：

```json
GET /bank/_search
{
  "query": {"match_all": {} }, 
  "aggs": {
    "city": {
      "terms": {
        "field": "age"
      },
      "aggs": {
        "avg_balance": {
            "avg": {
              "field": "balance"
            }
        }
      }
    }
  }
}
```

该查询为buckets中增加了metric，用于一些复杂计算

查询结果如下：

```json
"aggregations" : {
    "city" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 463,
      "buckets" : [
        {
          "key" : 31,
          "doc_count" : 61,
          "avg_balance" : {
            "value" : 28312.918032786885
          }
        },
        {
          "key" : 39,
          "doc_count" : 60,
          "avg_balance" : {
            "value" : 25269.583333333332
          }
        },
        {
          "key" : 26,
          "doc_count" : 59,
          "avg_balance" : {
            "value" : 23194.813559322032
          }
        },
        {
          "key" : 32,
          "doc_count" : 52,
          "avg_balance" : {
            "value" : 23951.346153846152
          }
        },
        {
          "key" : 35,
          "doc_count" : 52,
          "avg_balance" : {
            "value" : 22136.69230769231
          }
        },
        {
          "key" : 36,
          "doc_count" : 52,
          "avg_balance" : {
            "value" : 22174.71153846154
          }
        },
        {
          "key" : 22,
          "doc_count" : 51,
          "avg_balance" : {
            "value" : 24731.07843137255
          }
        },
        {
          "key" : 28,
          "doc_count" : 50,
          "avg_balance" : {
            "value" : 28754.58
          }
        },
        {
          "key" : 33,
          "doc_count" : 50,
          "avg_balance" : {
            "value" : 25093.94
          }
        },
        {
          "key" : 34,
          "doc_count" : 49,
          "avg_balance" : {
            "value" : 26809.95918367347
          }
        }
      ]
    }
  }
```

