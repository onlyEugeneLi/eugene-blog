# Palantir Infra/SRE 岗位的通关路线图与面试备战指南

## 🎯 针对 Palantir Infra/SRE 岗位的通关路线图
Palantir 非常看重防御性编程（Defensive Programming）思维（即假设所有可能出错的地方都会出错）和对分布式系统底层原理的理解。你可以分三个阶段来武装自己：
## 第一阶段：补齐语言与容器化短板（1-2个月）

* 捡起一门静态编译语言：
* Palantir 的底层技术栈主要使用 Java, Go, 或 Rust。Python 适合写胶水自动化脚本，但无法承载高并发的分布式内核。
   * 建议选择：Go 或 Java。如果想快速上手、拥抱云原生环境，选 Go（Kubernetes 是用 Go 写的）；如果想深入研究 Kafka、Spark、Flink 的源码，选 Java。
* 吃透 Docker 与 K8s 基础：
* 不要直接学 K8s。先用 Docker 把你之前写过的 FastAPI 应用打包成镜像（Image）。
   * 理解什么是容器隔离（Namespace, Cgroups）。
   * 在本地安装 minikube 或 kind，试着把你的 FastAPI 部署到本地 K8s 里，理解 Service、Pod、Deployment 的概念。

## 第二阶段：从“会用工具”到“理解底层原理”（3-5个月）
JD 里提到的 6 大工具不需要你全精通，但你必须挑出 2-3 个核心，理解它们在分布式架构下的核心痛点：

* Kubernetes (K8s) —— 必修课：
* 高级 SRE 不能只当 K8s 的“用户”，要当“管理员”。
   * 核心学习点：K8s 的声明式 API 哲学、控制循环（Control Loop）机制、网络模型（CNI）、持久化存储（CSI）。
   * 进阶实战：尝试用 Go 语言写一个简单的 K8s Operator（自定义控制器），这是大厂 Infra 团队的家常便饭。
* Kafka 与 Elasticsearch —— 分布式两件套：
* Kafka 的 SRE 视角：不是怎么写 Producer，而是如何保证消息不丢、不重（Exactly-Once）？当某个 Broker（节点）挂掉时，副本如何选举？什么是 Partition 的再平衡（Rebalance）？
   * Elasticsearch 的 SRE 视角：什么是倒排索引？写入数据时 Primary Shard 和 Replica Shard 是怎么同步的？脑裂（Brain-split）问题怎么解决？

## 第三阶段：建立 SRE 的监控与“防御性”思维（2-3个月）
Palantir 强调 Tracing（链路追踪）和 可观测性（Observability）。

* 学会“看病”：去 YouTube 搜索 Prometheus + Grafana 的配置教程。在你的 K8s 集群里部署它们，监控你那台 FastAPI 的 CPU、内存和响应时间。
* 理解分布式追踪（Distributed Tracing）：大厂的系统有成百上千个微服务，一个请求卡住了，你怎么知道是卡在 Cassandra 还是 Kafka？去了解 OpenTelemetry / Jaeger 的工作原理。

------------------------------
## 🛠 推荐一个可以写进简历的“高含金量”打怪项目
与其零散地学，不如在本地（或 GCP 免费额度里）亲手搭一个分布式数据监控流水线。这个项目能完美覆盖 Palantir 的技术栈，且非常符合 SRE 的调性：

   1. 数据源：用 Python 写一个脚本，模拟成千上万个用户在持续访问你的 FastAPI 应用，不断产生日志。
   2. 数据传输（Kafka）：将这些日志实时推送到一个 Kafka Cluster 中。
   3. 数据消费与分析（Flink/Spark）：写一个简单的消费程序（可以用 PySpark 或 Flink），实时对这些日志进行聚合（例如：统计每分钟 500 报错的次数）。
   4. 数据存储与检索（Elasticsearch）：把处理好的结构化日志存入 Elasticsearch，并用 Kibana 面板展示出来。
   5. 平台托管（Kubernetes）：将以上所有组件（FastAPI, Kafka, ES）通过编写 YAML 文件，全部部署在你本地的 K8s 集群中。
   6. SRE 演练（混沌工程）：这是最加分的一步！ 手动去 kill 掉 Kafka 的一个节点，或者掐断 Elasticsearch 的网络，观察系统会不会崩？日志会不会丢？如果没有丢，在面试时聊这段“故障排查与自愈”的经历，面试官会非常惊艳。

------------------------------
## 📚 备战 Palantir 面试的避坑指南

   1. LeetCode 不要放下：Palantir 虽然招 Infra/SRE，但第一轮通常是非常硬核的算法面试（甚至会考硬核的系统设计/并发编程）。既然你平时在用 Python 写 LeetCode，请继续保持，确保 Medium（中等）难度能稳定解出。
   2. 经典必读书籍：
   * 《Designing Data-Intensive Applications》（数据密集型应用系统设计，简称 DDIA）—— 这本书是神书，Infra/SRE 领域人手一本，读完它你就弄懂了 Kafka、Cassandra 和 ES 的底层大半设计逻辑。
      * Google 的《Site Reliability Engineering》（SRE 官方手册，网上有免费英文版）—— 帮你建立起高级 SRE 的思维模型（如 Error Budget、SLO/SLI）。
   
刚毕业一年多觉得要学的东西多是非常正常的，因为分布式系统本身就是软件工程的“深水区”。你可以把这个大厂目标拆解成一整年的长期计划。
针对上面的通关路线，你想先从哪一部分切入？我们可以先聊聊如何把你的 FastAPI 应用迁移并打包进 Docker 和本地 K8s，迈出云原生的第一步！

