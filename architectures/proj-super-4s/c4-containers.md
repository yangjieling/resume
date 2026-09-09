# Container — 4S 店销售 BDC 平台

按「接入 → 业务服务 → 异步与同步 → 数据」四层展开；箭头以自上而下为主，减少交叉。

```mermaid
flowchart TB
  Client["门店 Web / 移动端"]

  GW["API 网关<br/>鉴权 · 路由 · 限流"]

  subgraph Biz["业务微服务 · Spring Cloud Alibaba"]
    LeadSvc["线索服务<br/>线索池 · 分配规则"]
    BdcSvc["销售 BDC 服务<br/>跟进 · 回流 · 预警"]
    CallSvc["外呼服务<br/>外呼任务 · 通话记录"]
    CampSvc["市场活动服务<br/>活动 · 线索转化"]
    ReceptSvc["接待预约服务<br/>到店 · 试乘试驾"]
  end

  subgraph Async["异步与同步"]
    MQ["RocketMQ<br/>导出 · 跟进事件"]
    Canal["Canal<br/>订阅 MySQL binlog"]
  end

  subgraph Data["数据面"]
    MySQL[("MySQL<br/>ShardingSphere 分库")]
    Redis[("Redis<br/>热点与会话")]
    ES[("Elasticsearch<br/>检索与列表")]
    OSS["OSS<br/>导出文件 / 附件"]
  end

  Client --> GW
  GW --> LeadSvc
  GW --> BdcSvc
  GW --> CallSvc
  GW --> CampSvc
  GW --> ReceptSvc

  LeadSvc --> MySQL
  BdcSvc --> MySQL
  CallSvc --> MySQL
  CampSvc --> MySQL
  ReceptSvc --> MySQL

  BdcSvc --> Redis
  LeadSvc --> Redis
  BdcSvc --> MQ
  CallSvc --> MQ
  CampSvc --> OSS

  MySQL --> Canal
  Canal --> MQ
  MQ --> Redis
  MQ --> ES
```

## 要点

- **责任链**落在销售 BDC：下次跟进、预警、回流等分配规则挂在跟进流程上（图中不展开细节）。
- **Canal + MQ** 保证 MySQL → Redis / ES 的近实时一致，支撑检索与列表读路径。
- 导出走 MQ + OSS，避免大导出拖垮核心跟进链路。
