# Dynamic mapping
One of the most important features of Elasticsearch is that it tries to get out of your way and let you start exploring your data as quickly as possible. To index a document, you don’t have to first create an index, define a mapping type, and define your fields — you can just index a document and the index, type, and fields will display automatically:（Elasticsearch最重要的特性之一是，它试图摆脱你的方式，让你尽快开始探索你的数据。要索引一个文档，你不需要先创建一个索引，定义一个映射类型，定义你的字段——你只需要索引一个文档，索引、类型和字段就会自动显示出来：）

```json
PUT data/_doc/1 
{ "count": 5 }

## 返回值如下:
   {
     "_index": "data",
     "_id": "1",
     "_version": 1,
     "result": "created",
     "_shards": {
       "total": 2,
       "successful": 1,
       "failed": 0
     },
     "_seq_no": 0,
     "_primary_term": 1
   }

# Creates the data index, the _doc mapping type, and a field called count with data type long.

  GET /data/_search
  {
    "query": {
      "match_all": {}
    }
  }
  # 查询响应如下:
  {
    "took": 1,
    "timed_out": false,
    "_shards": {
      "total": 1,
      "successful": 1,
      "skipped": 0,
      "failed": 0
    },
    "hits": {
      "total": {
        "value": 1,
        "relation": "eq"
      },
      "max_score": 1,
      "hits": [
        {
          "_index": "data",
          "_id": "1",
          "_score": 1,
          "_source": {
            "count": 5
          }
        }
      ]
    }
  }

```

The automatic detection and addition of new fields is called dynamic mapping. The dynamic mapping rules can be customized to suit your purposes with:(自动检测和添加新字段称为动态映射。动态映射规则可以根据您的目的进行定制：)
1. [Dynamic field mappings]
2. [Dynamic templates]()



