# AI Coding Agent 编排工作台 — 架构

内部形态：**hb-cli**（对接京东 Hiboos / JoyCode）  
开源同源：**[agent-desk](https://github.com/yangjieling/agent-desk)**（Apache-2.0）

本地优先的 Coding Agent 编排工作台：CLI + 本地面板负责任务生命周期、技能分发、工作流编排与人工卡点；通过可插拔 Provider 协同 JoyCode / Claude Code / Codex / Cursor 等 Agent，并经飞书 / 钉钉 / Webhook 完成深链确认。

## 图索引

| 层级 | 文件 | 看什么 |
|------|------|--------|
| Context | [c4-context.md](./c4-context.md) · [context.svg](./context.svg) | 人、本系统、外部 Agent / 通知 / 分发系统边界；**context.svg 为主展示** |
| Container | [c4-containers.md](./c4-containers.md) · [containers.svg](./containers.svg) | CLI、面板、执行器、SQLite、技能与 Provider；**containers.svg 为主展示** |
| Deployment | [c4-deployment.md](./c4-deployment.md) · [deploy.svg](./deploy.svg) | 开发机本地运行时 + 内网分发 / 开源演示拓扑；**deploy.svg 为主展示** |

建议阅读顺序：Context → Container → Deployment。
