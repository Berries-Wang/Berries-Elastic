# Mapping
Mapping is the process of defining how a document, and the fields it contains, are stored and indexed.（映射是定义文档及其所含字段的存储和索引方式的过程。）

Each document is a collection of fields, which each have their own data type. When mapping your data, you create a mapping definition, which contains a list of fields that are pertinent to the document. A mapping definition also includes metadata fields, like the _source field, which customize how a document’s associated metadata is handled.（每个文档都是字段的集合，每个字段都有自己的数据类型。映射数据时，您可以创建一个映射定义，其中包含与文档相关的字段列表。映射定义还包括元数据字段，例如 _source 字段，它自定义了如何处理文档的关联元数据。）

Use dynamic mapping and explicit mapping to define your data. Each method provides different benefits based on where you are in your data journey. For example, explicitly map fields where you don’t want to use the defaults, or to gain greater control over which fields are created. You can then allow Elasticsearch to add other fields dynamically.（使用动态映射和显式映射来定义数据。每种方法根据您在数据旅程中所处的位置提供不同的好处。例如，显式映射您不想使用默认值的字段，或者更好地控制要创建哪些字段。然后，您可以允许 Elasticsearch 动态添加其他字段。）

## 参考
1. [Mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html)