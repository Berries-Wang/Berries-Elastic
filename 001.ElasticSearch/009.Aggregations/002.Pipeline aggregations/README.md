# Pipeline aggregations
## 怎么理解 -- Pipeline aggregations
**Pipeline Aggregations（管道聚合）** 是 Elasticsearch 中一种特殊的聚合类型，它允许对其他聚合的结果进行进一步处理。与 Bucket Aggregations 和 Metrics Aggregations 不同，Pipeline Aggregations 并不直接操作原始文档数据，而是对已有的聚合结果进行二次计算或转换。

### 核心概念
1. **Pipeline（管道）**：
   - Pipeline Aggregations 依赖于其他聚合的结果，因此它们需要“管道”来接收输入数据。
   - 例如，计算某个聚合结果的导数、移动平均值或累积和。

2. **输入和输出**：
   - 输入：其他聚合的结果（如 Bucket Aggregations 或 Metrics Aggregations）。
   - 输出：对输入数据的进一步计算结果。

3. **类型**：
   - **Parent Pipeline Aggregations**：对父聚合的结果进行计算。
   - **Sibling Pipeline Aggregations**：对同级聚合的结果进行计算。

---

### 常见的 Pipeline Aggregations
以下是 Elasticsearch 中常用的 Pipeline Aggregations：

#### 1. **Derivative Aggregation（导数聚合）**
   - 计算某个指标的变化率（导数）。
   - 示例：计算每月销售额的变化率。
   ```json
   {
     "aggs": {
       "sales_over_time": {
         "date_histogram": {
           "field": "date",
           "calendar_interval": "month"
         },
         "aggs": {
           "total_sales": {
             "sum": { "field": "price" }
           },
           "sales_derivative": {
             "derivative": {
               "buckets_path": "total_sales"
             }
           }
         }
       }
     }
   }
   ```

#### 2. **Moving Function Aggregation（移动函数聚合）**
   - 对某个指标应用移动窗口函数（如移动平均值）。
   - 示例：计算每月销售额的 3 个月移动平均值。
   ```json
   {
     "aggs": {
       "sales_over_time": {
         "date_histogram": {
           "field": "date",
           "calendar_interval": "month"
         },
         "aggs": {
           "total_sales": {
             "sum": { "field": "price" }
           },
           "moving_avg": {
             "moving_fn": {
               "buckets_path": "total_sales",
               "window": 3,
               "script": "MovingFunctions.unweightedAvg(values)"
             }
           }
         }
       }
     }
   }
   ```

#### 3. **Cumulative Sum Aggregation（累积和聚合）**
   - 计算某个指标的累积和。
   - 示例：计算每月销售额的累积和。
   ```json
   {
     "aggs": {
       "sales_over_time": {
         "date_histogram": {
           "field": "date",
           "calendar_interval": "month"
         },
         "aggs": {
           "total_sales": {
             "sum": { "field": "price" }
           },
           "cumulative_sales": {
             "cumulative_sum": {
               "buckets_path": "total_sales"
             }
           }
         }
       }
     }
   }
   ```

#### 4. **Bucket Script Aggregation（桶脚本聚合）**
   - 使用脚本对多个指标进行计算。
   - 示例：计算每月销售额与成本的差值。
   ```json
   {
     "aggs": {
       "sales_over_time": {
         "date_histogram": {
           "field": "date",
           "calendar_interval": "month"
         },
         "aggs": {
           "total_sales": {
             "sum": { "field": "price" }
           },
           "total_cost": {
             "sum": { "field": "cost" }
           },
           "profit": {
             "bucket_script": {
               "buckets_path": {
                 "sales": "total_sales",
                 "cost": "total_cost"
               },
               "script": "params.sales - params.cost"
             }
           }
         }
       }
     }
   }
   ```

#### 5. **Bucket Selector Aggregation（桶选择器聚合）**
   - 根据条件筛选桶。
   - 示例：筛选出销售额大于 1000 的月份。
   ```json
   {
     "aggs": {
       "sales_over_time": {
         "date_histogram": {
           "field": "date",
           "calendar_interval": "month"
         },
         "aggs": {
           "total_sales": {
             "sum": { "field": "price" }
           },
           "sales_bucket_filter": {
             "bucket_selector": {
               "buckets_path": {
                 "total_sales": "total_sales"
               },
               "script": "params.total_sales > 1000"
             }
           }
         }
       }
     }
   }
   ```

---

### Pipeline Aggregations 的使用场景
1. **时间序列分析**：
   - 对时间序列数据进行变化率、移动平均值、累积和等计算。
   - 示例：计算每月销售额的移动平均值。

2. **复杂计算**：
   - 对多个指标进行组合计算。
   - 示例：计算利润（销售额 - 成本）。

3. **数据筛选**：
   - 根据条件筛选聚合结果。
   - 示例：筛选出销售额大于某个阈值的桶。

---

### 总结
- **Pipeline Aggregations** 是对其他聚合结果进行二次计算的聚合类型。
- 常见的 Pipeline Aggregations 包括 `derivative`、`moving_fn`、`cumulative_sum`、`bucket_script`、`bucket_selector` 等。
- 它们通常用于时间序列分析、复杂计算和数据筛选等场景。

通过 Pipeline Aggregations，可以更灵活地对聚合结果进行处理，满足复杂的分析需求。