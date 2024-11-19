# Lucene
Lucene是一个高效的，基于Java的全文检索库。

Apache Lucene™ is a high-performance, full-featured search engine library written entirely in Java. It is a technology suitable for nearly any application that requires structured search, full-text search, faceting, nearest-neighbor search across high-dimensionality vectors, spell correction or query suggestions.
> Apache Lucene™ 是一个高性能、功能齐全的搜索引擎库，完全用 Java 编写。该技术适用于几乎所有需要结构化搜索、全文搜索、分面、跨高维向量的最近邻搜索、拼写纠正或查询建议的应用程序。

## 什么是全文搜索
生活中的数据分为两部分: **结构化数据**  和  **非结构化数据**
- 结构化数据： 指具有固定格式或有限长度的数据，如数据库，元数据等
- 非结构化数据（*全文数据*）: 指不定长或无固定格式的数据，如 邮件 word文档等
- 半结构化数据： XML ， HTML ，当根据需要可按结构化数据来处理，也可提取出纯文本按非结构化数据来处理。

按照数据的分类，搜索也分为两种:
1. 对结构化数据的搜索: 如对数据库的搜索，用SQL语句。再对元数据的搜索，如利用Windows搜索对文件名、类型、修改时间进行搜索等
2. 对非结构化数据的搜索： 如利用Windows的搜索也可搜索文件内容，Linux下的grep命令，用Google和百度可以搜索大量内容数据

对非结构化数据（全文数据）搜索主要有两种方式:
1. 顺序扫描法: 例如要找内容包含某一个字符串的文件，就是一个文档一个文档地看，对于每一个文件，从头看到尾，如果此文档包含此字符串，则此文档为我们要找的文档，接着看下一个文档，直到扫描完所有的文档。
2. 全文检索
    1. 导引: 对非结构化数据顺序扫描很慢，对结构化数据的搜索却相对较快(由于结构化数据有一定的结构可以采取一定的搜索算法加快速度)，那么将非结构化数据想办法弄得有一定结构不就行了?
    2. Next: 这种想法构成了全文检索的基本思路，也即将对非结构化数据中的一部分信息提取出来，重新组织，使其变得有一定结构，然后对此有一定结构的数据进行搜索，从而达到搜索较快的目的。
    3. 从非结构化数据中提取出的然后重新组织的信息 ，称之为 索引
    4. **这种先建立索引，再对索引进行搜索的过程就叫全文检索（Full-Text Search）**












---

## 参考资料
1. [https://lucene.apache.org/core/index.html](https://lucene.apache.org/core/index.html)