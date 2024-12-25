# Runtime Fields
A runtime field is a field that is evaluated at query time. Runtime fields enable you to:(运行时字段是在查询时评估的字段。运行时字段可以让你:)

- Add fields to existing documents without reindexing your data(向现有文档添加字段而无需重新索引数据)
- Start working with your data without understanding how it’s structured(开始处理数据而无需了解其结构)
- Override the value returned from an indexed field at query time(在查询时覆盖索引字段返回的值)
- Define fields for a specific use without modifying the underlying schema(定义用于特定用途的字段而无需修改底层架构)

You access runtime fields from the search API like any other field, and Elasticsearch sees runtime fields no differently. You can define runtime fields in the index mapping or in the search request. Your choice, which is part of the inherent flexibility of runtime fields.(你可以像访问其他任何字段一样访问运行时字段。你可以在index mapping 或 search request中定义运行时字段。您的选择，这是运行时字段固有灵活性的一部分。)

## Benefits
Because runtime fields aren’t indexed<sup>运行时字段未被编入索引，所以在查询时需要全面扫描</sup>, adding a runtime field doesn’t increase the index size. You define runtime fields directly in the index mapping, saving storage costs and increasing ingestion speed. You can more quickly ingest data into the Elastic Stack and access it right away. When you define a runtime field, you can immediately use it in search requests, aggregations, filtering, and sorting.（由于运行时字段未编入索引，因此添加运行时字段不会增加索引大小。您可以直接在索引映射中定义运行时字段，从而节省存储成本并提高提取速度。您可以更快地将数据提取到 Elastic Stack 中并立即访问。定义运行时字段后，您可以立即将其用于搜索请求、聚合、过滤和排序。）


## Compromises <sup>妥协</sup>
Runtime fields use less disk space and provide flexibility in how you access your data, but can impact search performance based on the computation defined in the runtime script.(运行时字段使用较少的磁盘空间，并为您提供访问数据的灵活性，但可能会根据运行时脚本中定义的计算影响搜索性能。)

To balance search performance and flexibility, index fields that you’ll frequently search for and filter on, such as a timestamp. Elasticsearch automatically uses these indexed fields first when running a query, resulting in a fast response time. You can then use runtime fields to limit the number of fields that Elasticsearch needs to calculate values for. Using indexed fields in tandem with runtime fields provides flexibility in the data that you index and how you define queries for other fields.(为了平衡搜索性能和灵活性，请索引您经常搜索和过滤的字段，例如时间戳。Elasticsearch 在运行查询时会自动首先使用这些索引字段，从而缩短响应时间。然后，您可以使用运行时字段来限制 Elasticsearch 需要计算值的字段数量。将索引字段与运行时字段结合使用，可以为您提供索引数据和定义其他字段查询的灵活性。)


## 注意事项
### 1. 在查询时，运行时字段会覆盖掉映射字段
If you create a runtime field with the same name as a field that already exists in the mapping, the runtime field shadows the mapped field. At query time, Elasticsearch evaluates the runtime field, calculates a value based on the script, and returns the value as part of the query. Because the runtime field shadows the mapped field, you can override the value returned in search without modifying the mapped field.(如果您创建的运行时字段与映射中已存在的字段同名，则运行时字段会遮盖映射字段。在查询时，Elasticsearch 会评估运行时字段，根据脚本计算值，并将该值作为查询的一部分返回。由于运行时字段会遮盖映射字段，因此您可以覆盖搜索中返回的值，而无需修改映射字段。)




---

## 参考
1. [Runtime Fields](https://www.elastic.co/guide/en/elasticsearch/reference/current/runtime.html)