# 4S 店销售 BDC 平台 — 架构

面向汽车经销商 / 4S 店的销售数字化 SaaS：线索获取、BDC 外呼跟进、市场活动、到店接待与试乘试驾预约等销售前链路。

技术栈概览：Spring Cloud Alibaba · ShardingSphere · RocketMQ · MySQL · Redis · Elasticsearch · Canal · OSS · Docker / K8s（阿里云）

## 图索引

| 层级 | 文件 | 看什么 |
|------|------|--------|
| Context | [c4-context.md](./c4-context.md) | 门店角色、本平台与外部获客 / 通信系统 |
| Container | [c4-containers.md](./c4-containers.md) | 网关、业务微服务、中间件与数据面 |
| Deployment | [c4-deployment.md](./c4-deployment.md) · [deploy.svg](./deploy.svg) | 阿里云容器化部署拓扑；**deploy.svg 为主展示** |

建议阅读顺序：Context → Container → Deployment。
