# Metrics aggregations
The aggregations in this family compute metrics based on values extracted in one way or another from the documents that are being aggregated. The values are typically extracted from the fields of the document (using the field data), but can also be generated using scripts.(这个系列中的聚合基于以某种方式从被聚合的文档中提取的值来计算度量。这些值通常从文档的字段中提取（使用字段数据），但也可以使用脚本生成。)

Numeric metrics aggregations are a special type of metrics aggregation which output numeric values. Some aggregations output a single numeric metric (e.g. avg) and are called single-value numeric metrics aggregation, others generate multiple metrics (e.g. stats) and are called multi-value numeric metrics aggregation. The distinction between single-value and multi-value numeric metrics aggregations plays a role when these aggregations serve as direct sub-aggregations of some bucket aggregations (some bucket aggregations enable you to sort the returned buckets based on the numeric metrics in each bucket).(数值指标聚合是一种特殊类型的指标聚合，它输出数值。一些聚合输出单个数字指标（例如avg），称为 single-value numeric metrics aggregation ，其他聚合生成多个指标（例如stats），称为 multi-value numeric metrics aggregation 。单值和多值数值指标聚合之间的区别在这些聚合作为某些存储桶聚合的直接子聚合时发挥作用（某些存储桶聚合允许您根据每个存储桶中的数值指标对返回的存储桶进行排序）。)

## 怎么理解 Metrics aggregations -- deepseek
**Metrics Aggregations（指标聚合）** 是 Elasticsearch 中用于对文档进行计算和统计的聚合类型。与 Bucket Aggregations（桶聚合）不同，Metrics Aggregations 不是将文档分组，而是对字段的值进行数学计算，例如求和、平均值、最大值、最小值等。

### 核心概念
1. **Metrics（指标）**：
   - 指标是对字段值的计算结果，通常是数值类型的字段。
   - 例如，计算某个字段的平均值、总和、最大值等。

2. **单值指标 vs 多值指标**：
   - **单值指标**：返回一个值，例如 `avg`（平均值）、`sum`（总和）。
   - **多值指标**：返回多个值，例如 `stats`（统计信息，包括平均值、最大值、最小值等）。

3. **与 Bucket Aggregations 结合**：
   - Metrics Aggregations 通常与 Bucket Aggregations 结合使用，用于对每个桶中的文档进行计算。
   - 例如，按商品类别分组后，计算每个类别的平均价格。

---

### 常见的 Metrics Aggregations
以下是 Elasticsearch 中常用的 Metrics Aggregations：

#### 1. **Avg Aggregation（平均值聚合）**
   - 计算字段的平均值。
   - 示例：计算商品价格的平均值。
   ```json
   {
     "aggs": {
       "avg_price": {
         "avg": { "field": "price" }
       }
     }
   }
   ```

#### 2. **Sum Aggregation（求和聚合）**
   - 计算字段的总和。
   - 示例：计算所有商品价格的总和。
   ```json
   {
     "aggs": {
       "total_sales": {
         "sum": { "field": "price" }
       }
     }
   }
   ```

#### 3. **Max Aggregation（最大值聚合）**
   - 计算字段的最大值。
   - 示例：查找最高价格的商品。
   ```json
   {
     "aggs": {
       "max_price": {
         "max": { "field": "price" }
       }
     }
   }
   ```

#### 4. **Min Aggregation（最小值聚合）**
   - 计算字段的最小值。
   - 示例：查找最低价格的商品。
   ```json
   {
     "aggs": {
       "min_price": {
         "min": { "field": "price" }
       }
     }
   }
   ```

#### 5. **Stats Aggregation（统计聚合）**
   - 返回字段的统计信息，包括 `count`（数量）、`min`（最小值）、`max`（最大值）、`avg`（平均值）、`sum`（总和）。
   - 示例：获取商品价格的统计信息。
   ```json
   {
     "aggs": {
       "price_stats": {
         "stats": { "field": "price" }
       }
     }
   }
   ```

#### 6. **Extended Stats Aggregation（扩展统计聚合）**
   - 在 `stats` 的基础上，增加 `sum_of_squares`（平方和）、`variance`（方差）、`std_deviation`（标准差）等统计信息。
   - 示例：获取商品价格的扩展统计信息。
   ```json
   {
     "aggs": {
       "price_extended_stats": {
         "extended_stats": { "field": "price" }
       }
     }
   }
   ```

#### 7. **Cardinality Aggregation（基数聚合）**
   - 计算字段的唯一值数量（去重计数）。
   - 示例：统计有多少个不同的商品类别。
   ```json
   {
     "aggs": {
       "unique_categories": {
         "cardinality": { "field": "category.keyword" }
       }
     }
   }
   ```

#### 8. **Value Count Aggregation（值计数聚合）**
   - 计算字段的非空值数量。
   - 示例：统计有多少个商品有价格信息。
   ```json
   {
     "aggs": {
       "price_count": {
         "value_count": { "field": "price" }
       }
     }
   }
   ```

---

### Metrics Aggregations 与 Bucket Aggregations 结合使用
Metrics Aggregations 通常与 Bucket Aggregations 结合使用，用于对每个桶中的文档进行计算。

#### 示例：按商品类别分组，并计算每个类别的平均价格
```json
{
  "aggs": {
    "product_categories": {
      "terms": { "field": "category.keyword" },
      "aggs": {
        "avg_price": {
          "avg": { "field": "price" }
        }
      }
    }
  }
}
```

- **`product_categories`**：按商品类别分组（Bucket Aggregation）。
- **`avg_price`**：计算每个类别的平均价格（Metrics Aggregation）。

---

### 总结
- **Metrics Aggregations** 用于对字段的值进行数学计算，例如求和、平均值、最大值、最小值等。
- 常见的 Metrics Aggregations 包括 `avg`、`sum`、`max`、`min`、`stats`、`cardinality` 等。
- Metrics Aggregations 通常与 Bucket Aggregations 结合使用，用于对每个桶中的文档进行计算。

通过 Metrics Aggregations，可以快速对数据进行统计分析，适用于各种数值计算场景。

