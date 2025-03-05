# Full text queries
The full text queries enable you to search analyzed text fields such as the body of an email. The query string is processed using the same analyzer that was applied to the field during indexing.(全文查询使您能够搜索分析过的文本字段，如电子邮件正文。查询字符串使用与索引期间应用于字段的分析器相同的分析器进行处理。)
> "analyzed"，见Lucene索引过程

全文搜索包含:
+ intervals query
  - A full text query that allows fine-grained control of the ordering and proximity of matching terms.(一种全文查询，允许对匹配术语的顺序和接近度进行细粒度控制。)
+ match query
  - The standard query for performing full text queries, including fuzzy matching and phrase or proximity queries.(用于执行全文查询的标准查询，包括模糊匹配和短语或邻近查询。)
+ match_bool_prefix query
  - Creates a bool query that matches each term as a term query, except for the last term, which is matched as a prefix query（创建一个bool查询，将每个词作为词查询进行匹配，但最后一个词作为前缀查询进行匹配）
+ match_phrase query
  - Like the match query but used for matching exact phrases or word proximity matches.（类似于匹配查询，但用于匹配精确的短语或单词邻近匹配。）
+ match_phrase_prefix query
  - Like the match_phrase query, but does a wildcard search on the final word.（与match_phrase查询类似，但对最后一个单词进行通配符搜索。）
+ multi_match query
  - The multi-field version of the match query.
+ combined_fields query
  - Matches over multiple fields as if they had been indexed into one combined field.
+ query_string query
  - Supports the compact Lucene query string syntax, allowing you to specify AND|OR|NOT conditions and multi-field search within a single query string. For expert users only.
+ simple_query_string query
  - A simpler, more robust version of the query_string syntax suitable for exposing directly to users.(一个更简单、更健壮的query_string语法版本，适合直接向用户公开。)

## 参考资料
1. [Full Text queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html)