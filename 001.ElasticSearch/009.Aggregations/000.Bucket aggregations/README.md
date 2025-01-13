# Bucket aggregations
Bucket aggregations don’t calculate metrics over fields like the metrics aggregations do, but instead, they create buckets of documents. Each bucket is associated with a criterion (depending on the aggregation type) which determines whether or not a document in the current context "falls" into it. In other words, the buckets effectively define document sets. In addition to the buckets themselves, the bucket aggregations also compute and return the number of documents that "fell into" each bucket.(桶聚合不像指标聚合那样计算字段上的指标，而是创建文档桶。每个bucket都与一个标准相关联（取决于聚合类型），该标准决定当前上下文中的文档是否“属于”它。换句话说，存储桶有效地定义了文档集。除了桶本身，桶聚合还计算并返回“落入”每个桶的文档的数量。)

Bucket aggregations, as opposed to metrics aggregations, can hold sub-aggregations. These sub-aggregations will be aggregated for the buckets created by their "parent" bucket aggregation.(与度量聚合相反，桶聚合可以保存子聚合。这些子聚合将为它们的“父”桶聚合创建的桶聚合。)

There are different bucket aggregators, each with a different "bucketing" strategy. Some define a single bucket, some define fixed number of multiple buckets, and others dynamically create the buckets during the aggregation process.(有不同的桶聚合器，每个都有不同的“桶”策略。有些定义单个桶，有些定义固定数量的多个桶，还有一些在聚合过程中动态创建桶。)

## 怎么理解桶聚合-来自deepseek
Elasticsearch 中的 **Bucket Aggregations** 用于将文档分组到不同的“桶”中，类似于 SQL 中的 `GROUP BY`。每个桶代表一组具有共同特征的文档。Bucket Aggregations 是 Elasticsearch 聚合框架的核心部分，常用于数据分析和统计。

### 核心概念
1. **Bucket（桶）**：
   - 桶是文档的集合，每个桶代表一组具有相同特征的文档。
   - 例如，按年龄段分组时，每个年龄段对应一个桶。

2. **Metric Aggregations（指标聚合）**：
   - 在 Bucket Aggregations 的基础上，可以对每个桶进行进一步计算，如求和、平均值等。

3. **Nested Aggregations（嵌套聚合）**：
   - 可以在一个 Bucket Aggregation 内嵌套另一个 Bucket Aggregation 或 Metric Aggregation，实现更复杂的分组和计算。

### 常见的 Bucket Aggregations
1. **Terms Aggregation**：
   - 按字段的值分组，适用于分类数据。
   - 示例：按商品类别分组统计数量。
   ```json
   {
     "aggs": {
       "product_categories": {
         "terms": { "field": "category.keyword" }
       }
     }
   }
   ```

2. **Range Aggregation**：
   - 按数值范围分组。
   - 示例：按价格区间分组。
   ```json
   {
     "aggs": {
       "price_ranges": {
         "range": {
           "field": "price",
           "ranges": [
             { "to": 50 },
             { "from": 50, "to": 100 },
             { "from": 100 }
           ]
         }
       }
     }
   }
   ```

3. **Date Range Aggregation**：
   - 按日期范围分组。
   - 示例：按订单日期范围分组。
   ```json
   {
     "aggs": {
       "order_dates": {
         "date_range": {
           "field": "order_date",
           "ranges": [
             { "to": "now-10d/d" },
             { "from": "now-10d/d", "to": "now" },
             { "from": "now" }
           ]
         }
       }
     }
   }
   ```

4. **Histogram Aggregation**：
   - 按固定间隔分组，适用于数值字段。
   - 示例：按价格每 50 个单位分组。
   ```json
   {
     "aggs": {
       "price_histogram": {
         "histogram": {
           "field": "price",
           "interval": 50
         }
       }
     }
   }
   ```

5. **Date Histogram Aggregation**：
   - 按固定时间间隔分组，适用于日期字段。
   - 示例：按月分组统计订单数量。
   ```json
   {
     "aggs": {
       "orders_over_time": {
         "date_histogram": {
           "field": "order_date",
           "calendar_interval": "month"
         }
       }
     }
   }
   ```

### 嵌套聚合示例
可以在 Bucket Aggregation 内嵌套 Metric Aggregation，例如计算每个商品类别的平均价格：
```json
{
  "aggs": {
    "product_categories": {
      "terms": { "field": "category.keyword" },
      "aggs": {
        "avg_price": { "avg": { "field": "price" } }
      }
    }
  }
}
```

### 总结
- **Bucket Aggregations** 用于将文档分组到不同的桶中。
- 常见的 Bucket Aggregations 包括 Terms、Range、Date Range、Histogram 和 Date Histogram。
- 可以嵌套 Metric Aggregations 或进一步的 Bucket Aggregations 进行更复杂的分析。

通过 Bucket Aggregations，可以高效地对数据进行分组和统计分析。