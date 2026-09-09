# 架构图索引

本目录存放各项目的系统 / 容器 / 部署架构说明。图使用 **Mermaid flowchart**（按 C4 分层组织），以便在 GitHub 文件预览中直接渲染成图，而不是只显示源码。

| 项目 | 说明 |
|------|------|
| [4S 店销售 BDC 平台](./proj-super-4s/) | 经销商销售 SaaS：线索 / 外呼 / 活动 / 接待（分层部署图） |
| [AI Coding Agent 编排工作台](./proj-hb-cli/) | hb-cli / agent-desk：本地优先 Agent 编排与人机协同 |

> 说明：Mermaid 的 `C4Context` / `C4Deployment` 在 GitHub 预览中支持不稳定，因此对外展示图统一用 flowchart；语义仍对齐 C4（Context → Container → Deployment）。
> 简历入口等部署图定稿后再加，当前仅维护本目录。
