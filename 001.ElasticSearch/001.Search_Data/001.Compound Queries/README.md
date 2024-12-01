# Compound Query
Compound queries wrap other compound or leaf queries, either to combine their results and scores, to change their behaviour, or to switch from query to filter context.(复合查询包装其他复合查询或叶查询，以组合其结果和分数，改变其行为，或从查询切换到过滤上下文。)

The queries in this group are:
+ [bool query](./001.Boolean%20Query.md)
  - The default query for combining multiple leaf or compound query clauses, as must, should, must_not, or filter clauses. The must and should clauses have their scores combined — the more matching clauses, the better — while the must_not and filter clauses are executed in filter context.

+ boosting query
  - Return documents which match a positive query, but reduce the score of documents which also match a negative query.(返回与肯定查询匹配的文档，但降低与否定查询匹配的文档的分数。)

+ constant_score query
  - A query which wraps another query, but executes it in filter context. All matching documents are given the same “constant” _score.

+ dis_max query
  - A query which accepts multiple queries, and returns any documents which match any of the query clauses. While the bool query combines the scores from all matching queries, the dis_max query uses the score of the single best- matching query clause.(接受多个查询并返回与任何查询子句匹配的任何文档的查询。虽然 bool 查询会组合所有匹配查询的分数，但 dis_max 查询会使用单个最佳匹配查询子句的分数。)

+ function_score query
  - Modify the scores returned by the main query with functions to take into account factors like popularity, recency, distance, or custom algorithms implemented with scripting.(使用函数修改主查询返回的分数，以考虑受欢迎程度、新近度、距离或使用脚本实现的自定义算法等因素。)


## 参考资料
1. [https://www.elastic.co/guide/en/elasticsearch/reference/current/compound-queries.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/compound-queries.html)