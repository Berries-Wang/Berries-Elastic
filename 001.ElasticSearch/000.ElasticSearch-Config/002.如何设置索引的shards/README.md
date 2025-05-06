# Shard规划建议
Unfortunately, there is no one-size-fits-all sharding strategy. A strategy that works in one environment may not scale in another. A good sharding strategy must account for your infrastructure, use case, and performance expectations.(遗憾的是，没有放之四海而皆准的分片策略。在某个环境中有效的策略未必能扩展到另一个环境中。一个好的分片策略必须考虑到您的基础设施、用例和性能预期。)


### 一般调整准则
+ shard大小尽量在10GB ~ 50GB之间
+ 将每个shard上的文档数控制在2亿以下

### Shard调整注意事项
#### Searches run on a single thread per shard(每个shard在执行搜索时都是单线程在执行)
大多数搜索都会找到多个分片。每个分片在单个 CPU 线程上运行搜索。虽然一个分片可以运行多个并发搜索，但跨大量分片进行搜索可能会耗尽节点的搜索线程池 。这可能会导致吞吐量低和搜索速度慢
```txt
    查看: 001.ElasticSearch/012.ElasticSearch-IN-Production/000.Node负载过高/000.分片过多场景.md
  当分片过多，并发查询，会导致系统负载过高.
```

#### Each index,shard,segment and field has overhead(每个索引、分片、段和字段都有开销)
每个索引和每个分片都需要一些内存和 CPU 资源。在大多数情况下，一小部分大分片使用的资源比许多小分片少。

分段在分片的资源使用中起着重要作用。大多数分片包含多个分段，用于存储其索引数据。Elasticsearch 将一些 segment 元数据保存在堆内存中，以便可以快速检索以进行搜索。随着分片的增长，其分段会合并为更少、更大的分段。这减少了 Segment 的数量，这意味着保存在堆内存中的元数据更少。

每个映射的字段在内存使用和磁盘空间方面也会带来一些开销。默认情况下，Elasticsearch 会自动为其索引的每个文档中的每个字段创建一个映射，但您可以关闭此行为以[控制您的映射](https://www.elastic.co/docs/manage-data/data-store/mapping/explicit-mapping)


#### Elasticsearch automatically balances shards within a data tier(Elasticsearch 会自动平衡数据层内的分片)
集群的节点分为数据层 。在每个层中，Elasticsearch 会尝试将索引的分片分布到尽可能多的节点。当您添加新节点或节点失败时，Elasticsearch 会自动在该层的其余节点之间重新平衡索引的分片。

### Best Practices(最佳实践)
#### Delete indices , not document（删除索引，而不是分片）
已删除的文档不会立即从 Elasticsearch 的文件系统中删除。相反，Elasticsearch 会在每个相关分片上将文档标记为已删除。标记的文档将继续使用资源，直到在定期分段合并期间将其删除。

#### Aim for shards of up to 200M documents, or with sizes between 10GB and 50GB（目标是最大 200M 文档的分片，或大小在 10GB 到 50GB 之间的分片）
每个分片都会产生一些相关的开销，包括集群管理和搜索性能。搜索 1000 个 50MB 分片的成本要比搜索包含相同数据的单个 50GB 分片要贵得多。但是，非常大的分片也可能导致搜索速度变慢，并且在失败后需要更长的时间才能恢复。

#### Aim for 20 shards or fewer per GB of heap memory (目标是每GB堆内存20或更少的分片)



#### shard过大导致性能下降，考虑修复索引分片大小
> 执行前需要考虑磁盘空间是否足够
+ Split Index
+ Reindex

#### Master-eligible nodes should have at least 1GB of heap per 3000 indices(符合 Master 条件的节点每 3000 个索引应至少有 1GB 的堆)
作为一般经验法则，主节点上每 GB 堆的索引数应少于 3000 个。


## 参考资料
+ [Size your shards(调整分片大小)](https://www.elastic.co/docs/deploy-manage/production-guidance/optimize-performance/size-shards)
+ [Size your shards (pdf)](./999.REFS/)