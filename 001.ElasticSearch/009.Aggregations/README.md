# Aggregation(聚合)
Elasticsearch organizes aggregations into three categories:(Elasticsearch将聚合分为三类：)
1. Metric aggregations that calculate metrics, such as a sum or average, from field values.(从字段值计算指标（如总和或平均值）的指标聚合。)
    > 对分类的文档进行聚合,类比MySQL: sum , avg , min ,max ...

2. Bucket aggregations that group documents into buckets, also called bins, based on field values, ranges, or other criteria.(存储桶聚合，根据字段值、范围或其他条件将文档分组到存储桶（也称为箱）中。)
    > 类似于MySQL 中的 GROUP BY

3. Pipeline aggregations that take input from other aggregations instead of documents or fields.(管道聚合，从其他聚合而不是文档或字段获取输入。)
    > 对聚合数据进行处理
