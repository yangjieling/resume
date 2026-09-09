# 架构图索引

本目录存放各项目的系统 / 容器 / 部署架构说明。Context / Container 仍以 **Mermaid flowchart**（按 C4 分层）为主；已定稿的 Deployment 以正交 **`deploy.svg`** 为主视图（spine + bus），Markdown 内嵌展示。

| 项目 | 说明 |
|------|------|
| [4S 店销售 BDC 平台](./proj-super-4s/) | 经销商销售 SaaS：线索 / 外呼 / 活动 / 接待（`deploy.svg` 主部署图） |
| [AI Coding Agent 编排工作台](./proj-hb-cli/) | hb-cli / agent-desk：本地优先 Agent 编排（`deploy.svg` 主部署图） |

> 说明：Mermaid 的 `C4Context` / `C4Deployment` 在 GitHub 预览中支持不稳定；有 SVG 的项目以 `deploy.svg` 为部署主图，其余层级继续用 flowchart。语义仍对齐 C4（Context → Container → Deployment）。
> 简历入口等部署图定稿后再加，当前仅维护本目录。
