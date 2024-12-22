# Cluster-level shard allocation and routing settings
Shard allocation is the process of assigning shard copies to nodes. This can happen during initial recovery, replica allocation, rebalancing, when nodes are added to or removed from the cluster, or when cluster or index settings that impact allocation are updated. (分片分配是将分片副本分配给节点的过程。这可能发生在初始恢复、副本分配、重新平衡期间、在集群中添加或删除节点时，或者在更新影响分配的集群或索引设置时)

One of the main roles of the master is to decide which shards to allocate to which nodes, and when to move shards between nodes in order to rebalance the cluster.(主服务器的主要角色之一是决定将哪些分片分配给哪些节点，以及何时在节点之间移动分片以重新平衡集群。)

---

## 实际场景应用
### 1. 下线节点
+ 通过操作[分片分配过滤器](./000.Cluster-level%20shard%20allocation%20filtering.md)即可让分片从一个ES实例中迁移，迁移完成后即可下线节点

