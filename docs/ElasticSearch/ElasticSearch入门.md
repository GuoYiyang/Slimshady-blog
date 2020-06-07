# ElasticSearch + ik分词器 + Kibana

## ElasticSearch

#### Docker部署ElasticSearch镜像

```shell
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.6.2
```

ElasticSearch的默认端口是9200，我们把宿主环境9200端口映射到Docker容器中的9200端口，就可以访问到Docker容器中的ElasticSearch服务了，同时我们把这个容器命名为elasticsearch。

- 9300：集群节点间通讯接口
- 9200：客户端访问接口
- discovery.type=single-node ：表示单节点启动

#### 验证是否启动成功

访问 http://127.0.0.1:9200，返回以下内容，说明部署成功

```json
{
  "name" : "c10333fb1897",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "JoD93JHsS9-pbCQqmhBx2g",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

## ik分词器

Elasticsearch默认也能对中文进行分词。但是是按照每个字进行分词的，这种在实际应用里肯定达不到想要的效果，因此，我们安装ik分词器来进行中文分词。

#### ES内置的Analyzer分析器

ES自带了许多内置的Analyzer分析器，无需配置就可以直接在index中使用：

- 标准分词器（standard）：以单词边界切分字符串为terms，根据Unicode文本分割算法。它会移除大部分的标点符号，小写分词后的term，支持停用词。
- 简单分词器（simple）：该分词器会在遇到非字母时切分字符串，小写所有的term。
- 空格分词器（whitespace）：遇到空格字符时切分字符串，
- 停用词分词器（stop）：类似简单分词器，同时支持移除停用词。
- 关键词分词器（keyword）：无操作分词器，会输出与输入相同的内容作为一个single term。
- 模式分词器（pattern）：使用正则表达式讲字符串且分为terms。支持小写字母和停用词。
- 语言分词器（language）：支持许多基于特定语言的分词器，比如english或french。
- 签名分词器（fingerprint）：是一个专家分词器，会产生一个签名，可以用于去重检测。
- 自定义分词器：如果内置分词器无法满足你的需求，可以自定义custom分词器，根据不同的character filters，tokenizer，token filters的组合 。例如IK就是自定义分词器。

#### 安装ik分词器

```shell
#首先需要启动elasticsearch容器
#然后进入elasticsearch容器
docker exec -it elasticsearch /bin/bash

#列出所有文件
ls
LICENSE.txt  NOTICE.txt  README.asciidoc  bin  config  data  jdk  lib  logs  modules  plugins

#进入plugin目录
cd plugins/

#安装ik分词器，注意和elasticsearch版本对应
elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.6.2/elasticsearch-analysis-ik-7.6.2.zip

#重启下容器
docker restart elasticsearch

#查看当前目录下文件
ls
analysis-ik
cd analysis-ik/
ls
commons-codec-1.9.jar  commons-logging-1.2.jar  elasticsearch-analysis-ik-7.6.2.jar  httpclient-4.5.2.jar  httpcore-4.4.4.jar  plugin-descriptor.properties  plugin-security.policy
```

IK支持两种分词模式：

- ik_max_word: 会将文本做最细粒度的拆分，会穷尽各种可能的组合
- ik_smart: 会做最粗粒度的拆分

#### 验证分词插件是否安装成功

增加一个叫test001的索引

```shell
curl -X PUT http://localhost:9200/test001
```

成功返回 {"acknowledged":true,"shards_acknowledged":true,"index":"test001"}

**使用ik_smart分词**

```shell
curl -X POST \
'http://127.0.0.1:9200/test001/_analyze?pretty=true' \
-H 'Content-Type: application/json' \
-d '{"text":"我们是软件工程师","tokenizer":"ik_smart"}'
```

得到如下结果

```json
{
  "tokens" : [
    {
      "token" : "我们",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "是",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "软件",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "工程师",
      "start_offset" : 5,
      "end_offset" : 8,
      "type" : "CN_WORD",
      "position" : 3
    }
  ]
}
```

**使用ik_max_word分词**

```shell
curl -X POST \
'http://127.0.0.1:9200/test001/_analyze?pretty=true' \
-H 'Content-Type: application/json' \
-d '{"text":"我们是软件工程师","tokenizer":"ik_max_word"}'
```

得到如下结果：

```json
{
  "tokens" : [
    {
      "token" : "我们",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "是",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "软件工程",
      "start_offset" : 3,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "软件",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 3
    },
    {
      "token" : "工程师",
      "start_offset" : 5,
      "end_offset" : 8,
      "type" : "CN_WORD",
      "position" : 4
    },
    {
      "token" : "工程",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 5
    },
    {
      "token" : "师",
      "start_offset" : 7,
      "end_offset" : 8,
      "type" : "CN_CHAR",
      "position" : 6
    }
  ]
}
```

可以看到ik_max_word分词更精细。

#### 自定义分词词典

新建自己的词典只需要简单几步就可以完成：

1.  在`elasticsearch-7.2.6/config/analysis-ik/`目录增加一个`my.dic`

`.dic`为词典文件，其实就是简单的文本文件，词语与词语直接需要换行。注意是UTF8编码。

```shell
[root@c10333fb1897 elasticsearch]# cd config/
[root@c10333fb1897 config]# ls
analysis-ik             jvm.options        roles.yml
elasticsearch.keystore  log4j2.properties  users
elasticsearch.yml       role_mapping.yml   users_roles
[root@c10333fb1897 config]# cd analysis-ik/
[root@c10333fb1897 analysis-ik]# ls
IKAnalyzer.cfg.xml          extra_single_word_low_freq.dic  quantifier.dic
extra_main.dic              extra_stopword.dic              stopword.dic
extra_single_word.dic       main.dic                        suffix.dic
extra_single_word_full.dic  preposition.dic                 surname.dic
[root@c10333fb1897 analysis-ik]# touch my.dic
[root@c10333fb1897 analysis-ik]# echo 朝阳公园>my.dic
[root@c10333fb1897 analysis-ik]# cat my.dic
朝阳公园
```

2.  修改`elasticsearch-7.2.6/config/analysis-ik/IKAnalyzer.cfg.xml`文件

```shell
[root@c10333fb1897 analysis-ik]# vi IKAnalyzer.cfg.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <comment>IK Analyzer 扩展配置</comment>
    <!--用户可以在这里配置自己的扩展字典 -->
    <entry key="ext_dict">my.dic</entry>
     <!--用户可以在这里配置自己的扩展停止词字典-->
    <entry key="ext_stopwords"></entry>
    <!--用户可以在这里配置远程扩展字典 -->
    <!-- <entry key="remote_ext_dict">words_location</entry> -->
    <!--用户可以在这里配置远程扩展停止词字典-->
    <!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
```

- 扩展停止词字典，这个是用来辅助断句的，也就是IK分词器遇到这些词就认为前面的词语不会与这些词构成词语。
- 远程词典，远程词典的好处是支持热更新。词典格式和本地的一致，都是一行一个分词（换行符用 `\n`）。

3. 重启ES，进行测试，测试结果如下

   ```shell
   slimshady@MacBook-Pro ~ % curl -X POST \             
   > 'http://127.0.0.1:9200/test001/_analyze?pretty=true' \
   > -H 'Content-Type: application/json' \
   > -d '{"analyzer": "ik_smart","text": "去朝阳公园"}'
   {
     "tokens" : [
       {
         "token" : "去",
         "start_offset" : 0,
         "end_offset" : 1,
         "type" : "CN_CHAR",
         "position" : 0
       },
       {
         "token" : "朝阳公园",
         "start_offset" : 1,
         "end_offset" : 5,
         "type" : "CN_WORD",
         "position" : 1
       }
     ]
   }
   ```
   
   说明自定义词典生效了。如果有多个词典，使用英文分号隔开：

```xml
<entry key="ext_dict">my.dic;custom/single_word_low_freq.dic</entry>
```

## Kibana



#### Docker部署kibana镜像

```shell
docker run --name kibana --link=elasticsearch -p 5601:5601 docker.elastic.co/kibana/kibana:7.6.2
```

* --link=elasticsearch：连接elasticsearch
* --name kibana：命名为kibana

#### 验证是否启动成功

访问 http://127.0.0.1:5601，进入以下页面，说明部署成功

![image-20200506164013003](https://tva1.sinaimg.cn/large/007S8ZIlgy1gezridwvl3j31p70u07np.jpg)

