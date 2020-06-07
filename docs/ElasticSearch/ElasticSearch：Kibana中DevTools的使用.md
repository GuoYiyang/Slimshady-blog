# ElasticSearch：Kibana中DevTools的使用

## DevTools

注：ElasticSearch里面有 `index` 和 `type` 的概念：index称为索引，type为文档类型，一个index下面有多个type，每个type的字段可以不一样。这类似于关系型数据库的 database 和 table 的概念。

实际使用中建议一个`index`里面仅有一个`type`，名称可以和index一致，或者使用固定的`doc`。

#### 节点操作

##### 查看健康状态

```shell
GET /_cat/health?format=json
```

>   `format=json` 表示输出json格式，默认是文本格式。

健康状态有3种：

-   Green - 正常（集群功能齐全）
-   Yellow - 所有数据均可用，但尚未分配一些副本（群集功能齐全）
-   Red - 某些数据由于某种原因不可用（群集部分功能可用）

>   注意：当群集为红色时，它将继续提供来自可用分片的搜索请求，但您可能需要尽快修复它，因为存在未分配的分片。

##### 查看节点

```shell
GET /_cat/nodes?format=json
```

#### 索引

##### 创建index

```shell
PUT /customer
```

>   注：实际项目里一般是不会直接这样创建 index 的，这里仅为演示。一般都是通过创建 mapping 手动定义 index 或者自动生成 index 。

##### 删除index

```shell
DELETE /customer
```

>   注：删除索引会把数据一并删除。实际操作请谨慎。

##### 查看所有index

```shell
GET /_cat/indices?format=json
```

#### 增删改查

ES文档有一些缺省字段，称之为`Meta-Fields`，例如`_index`、`_type`、`_id`等，查询文档的时候会返回。

##### 按ID新增数据

type为doc：

```shell
PUT /customer/_doc/1
{
  "name": "John Doe"
}
PUT /customer/_doc/2
{
  "name": "yujc",
  "age":22
}
```

>   如果索引index不存在，直接新增数据也会同时创建index。

##### 按ID更新数据

```shell
PUT /customer/_doc/2
{
  "name": "yujc2"
}

POST /customer/_doc/1
{
  "name": "yujc2",
  "age":22
}
```

`name`字段会被修改，而且`_version`会被修改为2。**该操作实际是覆盖数据。**

##### 按ID查询数据

```shell
GET /customer/_doc/1
```

##### 直接新增数据

我们也可以不指定文档ID从而直接新增数据：

```shell
POST /customer/_doc
{
  "name": "yujc",
  "age":23
}
```

>   注意这里使用的动作是`POST`。`PUT`新增数据必须指定文档ID。

##### 更新部分字段

如果只是想更新指定字段，必须使用`POST`加参数的形式：

```shell
POST /customer/_doc/1/_update
{
  "doc":{"name": "John"}
}
```

其中`_update`表示更新。Json里`doc`必须有，否则会报错。

##### 增加字段

在已有的数据基础上增加一个`year`字段，不会覆盖已有数据

```shell
POST /customer/_doc/1/_update
{
  "doc":{"year": 2018}
}
```

也可以使用简单脚本执行更新。此示例使用脚本将年龄增加5：

```shell
POST /customer/doc/1/_update
{
  "script":"ctx._source.age+=5"
}
```

##### 按ID删除数据

```shell
DELETE /customer/_doc/1
```

##### 查询指定 Index 的 mapping

```shell
GET /customer/_mapping
```

#### 批量操作

##### 批量创建

```shell
POST /customer/_doc/_bulk
{"index":{"_id":"3"}}
{"name": "John Doe3" }
{"index":{"_id":"4"}}
{"name": "Jane Doe4" }
```

该操作会新增2条记录，其中文档第1行和第3行提供的是要操作的文档id，第2行和第4行是相应的源文档，即数据内容。这里对文档的操作是`index`，也可以是`create`，二者都是创建文档，只是如果文档已存在，`index`会覆盖，`create`会失败。

##### 批量更新、删除

```shell
POST /customer/_doc/_bulk
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
```

该操作会更新ID为1的文档，删除ID为2的文档。对于删除操作，之后没有相应的源文档，因为删除只需要删除文档的ID。

>   注意：批量操作如果某条失败了，并不影响下一条继续执行。

##### 按条件更新

```shell
curl -X POST 
http://127.0.0.1:9200/test/_doc/_update_by_query 
-H "Content-Type: application/json" 
-d '{"script":{"source":"ctx._source[\"is_pub\"]=1"},"query":{"match_all":{}}}'
```

这个示例的含义是将文档`test/_doc`的所有文档的`is_pub`字段设置为1。

##### 按条件删除

```shell
curl -X POST 
http://127.0.0.1:9200/test/doc/_delete_by_query 
-H "Content-Type: application/json" 
-d '{"query":{"bool":{"filter":{"range":{"id":{"gt":1661208}}}}}}'
```

这个示例的含义是将文档`test/doc`里字段 id 符合`id>1661208`的全部删除。

#### 全文检索

##### 返回字段含义

-   took – Elasticsearch执行搜索的时间（以毫秒为单位）
-   timed_out – 搜索是否超时
-   _shards – 搜索了多少个分片，以及搜索成功/失败分片的计数
-   hits – 搜索结果，是个对象
-   hits.total – 符合我们搜索条件的文档总数
-   hits.hits – 实际的搜索结果数组（默认为前10个文档）
-   hits.sort - 对结果进行排序（如果按score排序则没有该字段）
-   hits._score、max_score -分数
-   _index：索引库
-   _type：文档类型
-   _id：文档id
-   _source：文档的源数据

##### match查询

```shell
GET /bank/_search
{
    "query" : {
        "match" : { "address" : "Avenue" }
    }
}
```

curl:

```shell
curl -XGET -H "Content-Type: application/json" "localhost:9200/bank/_search?pretty" -d '{"query":{"match":{"address":"Avenue"}}}'
```

上述查询返回结果是`address`含有`Avenue`的结果。

##### term查询

```shell
GET /bank/_search
{
    "query" : {
        "term" : { "address" : "Avenue" }
    }
}
```

curl:

```shell
curl -XGET 
-H "Content-Type: application/json" "localhost:9200/bank/_search?pretty" 
-d '{"query":{"term":{"address":"Avenue"}}}'
```

上述查询返回结果是`address`**等于**`Avenue`的结果。

##### 分页(from/size)

分页使用关键字from、size，分别表示偏移量、分页大小。

```shell
GET /bank/_search
{
  "query": { "match_all": {} },
  "from": 0,
  "size": 2
}
```

from默认是0，size默认是10。

>   注意：ES的from、size分页不是真正的分页，称之为*浅分*页。from+ size不能超过`index.max_result_window` 默认为`10,000` 的索引设置。

##### 排序(sort)

字段排序关键字是sort。支持升序(asc)、降序(desc)。默认是对`_score`字段进行排序。

```shell
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ],
  "from":0,
  "size":10
}
```

##### 脚本排序

允许基于自定义脚本进行排序，这是一个示例：

```shell
GET bank/account/_search
{
    "query": { "range": { "age":  {"gt": 20} }},
    "sort" : {
        "_script" : {
            "type" : "number",
            "script" : {
                "lang": "painless",
                "source": "doc['account_number'].value * params.factor",
                "params" : {
                    "factor" : 1.1
                }
            },
            "order" : "asc"
        }
    }
}
```

上述查询是使用脚本进行排序：按``account_number * 1.1`的结果进行升序。其中`lang`指的是使用的脚本语言类型为`painless`。`painless`支持`Math.log`函数。

##### 过滤字段

默认情况下，ES返回所有字段。这被称为源（`_source`搜索命中中的字段）。如果我们不希望返回所有字段，我们可以只请求返回源中的几个字段。

```shell
GET /bank/_search
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}
```

通过`_source`关键字可以实现字段过滤。

##### 返回脚本字段

可以通过脚本动态返回新定义字段。示例：

```shell
GET bank/account/_search
{
    "query" : {
        "match_all": {}
    },
    "size":2,
    "script_fields" : {
        "age2" : {
            "script" : {
                "lang": "painless",
                "source": "doc['age'].value * 2"
            }
        },
        "age3" : {
            "script" : {
                "lang": "painless",
                "source": "params['_source']['age'] * params.factor",
                "params" : {
                    "factor"  : 2.0
                }
            }
        }
    }
}
```

结果：

```shell
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 1000,
    "max_score": 1,
    "hits": [
      {
        "_index": "bank",
        "_type": "account",
        "_id": "25",
        "_score": 1,
        "fields": {
          "age3": [
            78
          ],
          "age2": [
            78
          ]
        }
      },
      {
        "_index": "bank",
        "_type": "account",
        "_id": "44",
        "_score": 1,
        "fields": {
          "age3": [
            74
          ],
          "age2": [
            74
          ]
        }
      }
    ]
  }
}
```

>   注意：使用`doc['my_field_name'].value`比使用`params['_source']['my_field_name']`更快更效率，推荐使用。

##### AND查询

如果我们想同时查询符合A和B字段的结果，该怎么查呢？可以使用must关键字组合。

```shell
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}

GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "account_number":136 } },
        { "match": { "address": "lane" } },
        { "match": { "city": "Urie" } }
      ]
    }
  }
}
```

##### OR查询

ES使用should关键字来实现OR查询。

```shell
GET /bank/_search
{
  "query": {
    "bool": {
      "should": [
        { "match": { "account_number":136 } },
        { "match": { "address": "lane" } },
        { "match": { "city": "Urie" } }
      ]
    }
  }
}
```

##### AND取反查

`must_not`关键字实现了既不包含A也不包含B的查询。

```shell
GET /bank/_search
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
```

表示 address 字段需要符合既不包含 mill 也不包含 lane。

##### 布尔组合查询

我们可以组合 must 、should 、must_not 进行复杂的查询。

-   A AND NOT B

```shell
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": 40 } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}
```

相当于SQL：

```sql
select * from bank where age=40 and state!= "ID";
```

-   A AND (B OR C)

```shell
GET /bank/_search
{
    "query":{
        "bool":{
            "must":[
                {"match":{"age":39}},
                {"bool":
                				{"should":[
                            {"match":{"city":"Nicholson"}},
                            {"match":{"city":"Yardville"}}
                        ]}
                }
            ]
        }
    }
}
```

相当于SQL：

```sql
select * from bank where age=39 and (city="Nicholson" or city="Yardville");
```

##### 范围查询

```shell
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

相当于SQL：

```sql
select * from bank where balance between 20000 and 30000;
```

多字段范围查询：

```shell
GET /bank/_search
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "bool":{
          "must":[
            {"range": {"balance": {"gte": 20000,"lte": 30000}}},
            {"range": {"age": {"gte": 30}}}
            ]
        }
      }
    }
  }
}
```

相当于SQL：

```sql
select * from bank where (balance between 20000 and 30000) and (age > 30);
```

##### 聚合查询

```shell
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

相当于SQL：

```sql
SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC
```

##### 多重聚合

我们可以在聚合的基础上再进行聚合，例如求和、求平均值等等。

```sql
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

####  Mapping

Mapping类似于数据库中的表结构定义，主要作用如下：

-   定义Index下字段名（Field Name）
-   定义字段的类型，比如数值型，字符串型、布尔型等
-   定义倒排索引的相关配置，比如是否索引、记录postion等

Mapping完整的内容可以分为四部分内容：

-   字段类型(Field datatypes)
-   元字段(Meta-Fields)
-   Mapping参数配置(Mapping parameters)
-   动态Mapping(Dynamic Mapping)

