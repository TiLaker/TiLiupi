

- 团队名称：Ti 流批
- 作者：shirly，Cabinfever，bangbang，nicholasjiang
- 项目进展：暂无

代码仓库： https://github.com/TiStore/TiLiupi
## 项目介绍

Ti 流批 是搭建在 TiFlash 之上的流式湖仓系统，集成 Flink Table Store 在 TiFlash 之上添加了更多实时场景的能力。

## 背景&动机

TiFlash 是 TiDB HTAP 形态的关键组件，它是 TiKV 的列存扩展，在提供了良好的隔离性的同时，也兼顾了强一致性，目前无法提供流批一体的能力。Flink Table Store是一个数据湖存储，用于实时流式 Changelog 摄取 (比如来自 Flink CDC 的数据) 和高性能查询。Ti 流批基于 TiFlash 列式存储且拥有借助 ClickHouse 高效实现的协处理器层能力，集成 Flink Table Store 为 Streaming Warehouse 打造一款流批一体的存储，面向诸如离线数仓加速、实时数仓加强等流批混用的场景。

## 项目设计
![image](https://user-images.githubusercontent.com/1085697/196090736-154070bd-d294-44f4-9f93-a65942c97777.png)


Flink Table Store 是一个湖存储，以低成本无服务的方式存储大量数据。湖存储中通过Manifest 管理文件，每个 Bucket 中是一个 LSM Tree，在湖存储上也支持和 Kafka 的集成，让你一张表同时存储离线和实时数据。TiFlash 提供列式存储，且拥有借助 ClickHouse 高效实现的协处理器层，依赖 Multi-Raft 体系以 Region 为单位进行数据复制和分散，兼容 TiDB 与 TiSpark，对接 Flink 计算引擎以低消耗不阻塞 TiKV 写入的方式，实现 Flink Table Store 的 File Store 接口，打造一款适合 TiDB 的流批一体的存储。
