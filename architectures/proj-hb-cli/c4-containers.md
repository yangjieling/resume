# Container — AI Coding Agent 编排工作台

展示可独立部署 / 运行的技术积木：CLI、本地面板、任务与工作流引擎、本地存储、技能包与各类 Provider。

```mermaid
flowchart TB
  Eng["研发工程师"]

  subgraph Workbench["工作台边界 · hb-cli / agent-desk"]
    CLI["CLI<br/>Python / Node<br/>安装 · 任务 · 工作流入口"]
    UI["本地面板 Web UI<br/>TypeScript<br/>任务时间轴 · 卡点操作"]
    API["本地控制面 API<br/>Fastify / 等价 HTTP<br/>任务状态机 · Webhook"]
    Exec["执行器<br/>子进程管理 · 停止/续跑<br/>stdout/stderr 采集"]
    WF["工作流引擎<br/>YAML Workflow<br/>共享会话 / 独立并行"]
    Skills["技能注册表<br/>SKILL.md 包<br/>系统 / 个人模板"]
    DB[(SQLite<br/>任务 · 工作流 · 设置)]
  end

  subgraph Providers["可插拔 Provider"]
    AP["Agent Provider<br/>JoyCode / Claude / Codex / Cursor"]
    IP["Issue Provider<br/>缺陷平台适配"]
    NP["Notify Provider<br/>飞书 / 钉钉 / Webhook"]
  end

  Eng --> CLI
  Eng --> UI
  CLI --> API
  UI --> API
  API --> WF
  API --> Exec
  API --> Skills
  API --> DB
  WF --> Exec
  Exec --> AP
  API --> IP
  API --> NP
  Skills -.->|注入提示词与工作目录| Exec
```

## 要点

- **控制面与执行面分离**：API / 状态机可观测；执行器只负责拉起 Agent 子进程与回收。
- **Schema 优先**：任务、工作流、设置结构可交换，支撑内网 Python 形态与开源 TS monorepo 同源演进。
- **技能是一等公民**：以包形式分发，而不是写死在某一条流水线里。
