# Search Data
Elasticsearch supports several search methods:
+ Search for exact values
  - Search for [exact values or ranges](./002.Term-level%20queries.md) of numbers, dates, IPs, or strings.
+ Full-text search
  - Use [full text queries](./003.Full%20text%20queries.md) to query[ unstructured textual data](../05A.Mapping/000.Text%20analysis.md) and find documents that best match query terms.
+ Vector search
  - Store vectors in Elasticsearch and use approximate nearest neighbor (ANN) or k-nearest neighbor (kNN) search to find vectors that are similar, supporting use cases like semantic search.

## Run a Search
To run a search request, you can use the search API or Search Applications.
+ [Search API](./015A.The%20search%20API.md)
   - The search API enables you to search and aggregate data stored in Elasticsearch using a query language called the Query DSL.
+ Search Applications —— Kibana
   - Search Applications enable you to leverage the full power of Elasticsearch and its Query DSL, with a simplified user experience. Create search applications based on your Elasticsearch indices, build queries using search templates, and easily preview your results directly in the Kibana Search UI.(搜索应用程序可让您充分利用 Elasticsearch 及其查询 DSL 的全部功能，同时获得简化的用户体验。根据您的 Elasticsearch 索引创建搜索应用程序，使用搜索模板构建查询，并直接在 Kibana 搜索 UI 中轻松预览结果。)