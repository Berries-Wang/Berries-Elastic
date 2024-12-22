# Index Modules
阅读当前文件夹文档，可以发现如下实际使用案例:

## 实际场景使用案例
### 节点修改配置重启
因为心跳存在，所以，如果直接修改配置文件重启，可能会导致集群认为操作节点因故障离线，从而导致分片重分配等一系列操作。阅读[Delaying allocation when a node leaves](./002.Delaying%20allocation%20when%20a%20node%20leaves.md) 可以配置心跳时间处理。当然，操作完还是得恢复