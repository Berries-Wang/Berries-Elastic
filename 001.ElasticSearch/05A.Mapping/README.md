# Mapping Parameters
> 先阅读:[Lucene索引磁盘占用分析](../../999.Lessons/001.Lucene索引磁盘占用分析.md)

|参数名|类型|含义|使用示例|使用场景|
|-|-|-|-|-|
|enabled|Mapping parameters|Elasticsearch 会尝试为你提供的所有字段建立索引，但有时你只想存储字段而不为其建立索引。(`是否对该字段进行处理`)|{"mappings":{"properties":{"user_id":{"type":"keyword"},"last_updated":{"type":"date"},"session_data":{"type":"object","enabled":false}}}}|节省磁盘: `关闭后，只在_source中存储`,不会被解析，即存储时不会校验字段类型<br/>类似index与doc_value的总开关|
|index|Mapping parameters|控制是否为字段值编制索引,即是否加入倒排索引|{"mappings":{"properties":{"title":{"type":"text","index":true},"metadata":{"type":"object","enabled":false}}}}|节省磁盘: 只存储，不创建索引，`会被解析`: 不会加入到倒排索引中，但是会存储在_source , doc_values中，即`不可以`被搜索，但是`可以`被排序和聚合|
|doc_values|Mapping parameters|doc_values 字段是在文档索引时构建的磁盘上数据结构，可实现高效的数据访问。它存储与 _source 相同的值，但采用列式格式，以便更有效地进行排序和聚合。（`为了排序和聚合`）|{"mappings":{"properties":{"status_code":{"type":"keyword"},"session_id":{"type":"keyword","doc_values":false}}}}|节省磁盘: <br/>1. 会占用额外存储空间<br/>2.与_source独立，同时开启doc_values和_source则会将该字段原始内容保存两份<br/>3.数据在磁盘上采用列式存储<br/>4.关闭后无法使用排序和聚合|
|_source|Document metadata fields|原始 JSON 文档正文存储字段，保存post到API接口的原始json文档，查阅:[Lucene&ElasticSearch架构](../../999.Lessons/000.Lucene&ElasticSearch架构.md)发现，最后还是通过文档ID去查找原始文档内容||节省磁盘:<br/>1. 可以不存储该字段(当只关心一篇文档是否存在，而不关心这个文档的内容)<br/>2. 不存储无法获取原文<br/>3. 无法reindex <br/>4. 无法在查询中使用script|
|store|Mapping parameters|独立于_source,新存储一份|{"mappings":{"properties":{"title":{"type":"text","store":true},"date":{"type":"date","store":true},"content":{"type":"text"}}}}|加速查询效率，增大磁盘占用率<br/>如果_source中有长度很长的文本（如一篇文章）和较短的文本（如文章标题），当只需要取出标题时，如果使用_source字段，ES需要读取整个_source字段，然后返回其中的title，由此会引来额外的IO开销，降低效率。此时可以选择将title的store设置为true，在_source字段外单独存储一份。读取时不必在读取整个_source字段了|
|term_vector|Mapping parameters|空间换时间，加速搜索||加速查询效率，增大磁盘占用率:<br/>在对文本进行analyze的过程中，可以保留有关分词结果的相关信息，包括单词列表、单词之间的先后顺序、单词在原文中的位置等信息。查询结果返回的高亮信息就可以利用其中的数据来返回。默认情况下，term_vector是关闭的，如有需要（如加速highlight结果）可以开启该字段的存储。|
|analyzer|Mapping parameters|The analyzer parameter specifies the analyzer used for text analysis when indexing or searching a text field.(analyzer 参数指定在为文本字段编制索引或搜索文本字段时用于文本分析的分析器 。)--分词工具|{"settings":{"analysis":{"analyzer":{"my_analyzer":{"type":"custom","tokenizer":"standard","filter":["lowercase"]},"my_stop_analyzer":{"type":"custom","tokenizer":"standard","filter":["lowercase","english_stop"]}},"filter":{"english_stop":{"type":"stop","stopwords":"_english_"}}}},"mappings":{"properties":{"title":{"type":"text","analyzer":"my_analyzer","search_analyzer":"my_stop_analyzer","search_quote_analyzer":"my_analyzer"}}}}|text类型字段的索引建立 和 搜索|
|_routing field|Document metadata fields|请参考:[_routing field](./000.Document%20metadata%20fields/000.mapping-routing-field.png)||
|||||


---

## 参考资料
+ [https://github.com/elastic/elasticsearch](https://github.com/elastic/elasticsearch) 文档就在docs目录下