# Deployment — 4S 店销售 BDC 平台

部署图刻意做成**自上而下五层**：客户端 → 入口 → K8s 应用 → 中间件 → 数据存储。层与层之间单向连接，避免左右大框互穿。

```mermaid
flowchart TB
  subgraph L1["① 客户端"]
    Web["门店浏览器 / 移动端 H5"]
  end

  subgraph L2["② 接入层 · 阿里云"]
    SLB["负载均衡 SLB / ALB"]
    GW["API 网关 Pod"]
  end

  subgraph L3["③ 应用层 · Kubernetes"]
    Lead["线索服务"]
    BDC["销售 BDC"]
    Call["外呼服务"]
    Camp["市场活动"]
    Recept["接待预约"]
  end

  subgraph L4["④ 中间件"]
    Nacos["Nacos<br/>注册 / 配置"]
    MQ["RocketMQ"]
    Canal["Canal"]
  end

  subgraph L5["⑤ 数据与对象存储"]
    MySQL[("MySQL 分库集群<br/>ShardingSphere")]
    Redis[("Redis")]
    ES[("Elasticsearch")]
    OSS["对象存储 OSS"]
  end

  Web --> SLB
  SLB --> GW

  GW --> Lead
  GW --> BDC
  GW --> Call
  GW --> Camp
  GW --> Recept

  Lead --> Nacos
  BDC --> Nacos
  Call --> Nacos
  Camp --> Nacos
  Recept --> Nacos

  Lead --> MySQL
  BDC --> MySQL
  Call --> MySQL
  Camp --> MySQL
  Recept --> MySQL

  BDC --> Redis
  Lead --> Redis

  BDC --> MQ
  Call --> MQ
  Camp --> MQ
  Camp --> OSS

  MySQL --> Canal
  Canal --> MQ
  MQ --> Redis
  MQ --> ES
```

## 读图说明

| 层 | 职责 |
|----|------|
| 客户端 | 门店人员访问入口 |
| 接入 | SLB + 网关，统一鉴权与路由 |
| 应用 | 业务微服务，容器化跑在 K8s |
| 中间件 | 注册配置、消息、binlog 同步 |
| 数据 | 分库 MySQL、缓存、检索、对象存储 |

## 设计取舍

- **一层一行**，不用「左右大框对打」。
- **关系尽量竖直向下**；同步链路 `MySQL → Canal → MQ → Redis/ES` 单独成一条。
- 节点控制在一屏可读；金融/交车等后链路不进部署图（业务流程图里已有）。
