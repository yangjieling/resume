# Deployment — AI Coding Agent 编排工作台

部署视角：日常运行在**研发本机**；内网通过 Artifactory 分发安装包与技能；开源侧以 GitHub 仓库对外演示同源能力。

```mermaid
flowchart TB
  subgraph DevMachine["研发机 · macOS / Linux"]
    subgraph Runtime["本地运行时"]
      CLI["hb-cli / agent-desk CLI"]
      Panel["本地面板<br/>localhost Web UI"]
      API["本地控制面<br/>本机 HTTP 端口"]
      Exec["Agent 执行器<br/>子进程"]
      DB[(SQLite 文件<br/>用户数据目录)]
      SkillDir["技能与流程目录<br/>系统模板 + 个人"]
    end

    subgraph LocalAgents["本机已安装的 Agent CLI"]
      Joy["JoyCode"]
      Claude["Claude Code"]
      Codex["Codex"]
      Cursor["Cursor Agent"]
    end
  end

  subgraph Intranet["企业内网 · 可选"]
    Artif["Artifactory<br/>安装包 / 版本元数据"]
    Mono["Monolith / 内网门户<br/>一键安装入口"]
    Issue["缺陷平台"]
    IM["飞书 / 钉钉"]
  end

  subgraph Public["公网 · 开源形态"]
    GH["GitHub<br/>yangjieling/agent-desk"]
    Pages["文档与演示说明"]
  end

  Mono -->|安装脚本| Artif
  Artif -->|拉取安装包 / 自检升级| CLI
  Panel --> API
  CLI --> API
  API --> DB
  API --> SkillDir
  API --> Exec
  Exec --> Joy
  Exec --> Claude
  Exec --> Codex
  Exec --> Cursor
  API <-->|缺陷拉取 / 回写| Issue
  API -->|卡点通知| IM
  IM -->|深链继续 / 中止| API
  GH -.->|同源能力对外| Pages
  SkillDir -.->|Skill 种子同步| GH
```

## 部署说明

| 区域 | 跑什么 | 说明 |
|------|--------|------|
| 研发机 | CLI、面板、控制面、执行器、SQLite | 默认形态；数据与日志留在本地 |
| 本机 Agent CLI | JoyCode 等 | 由执行器按 Provider 拉起，不托管在远端集群 |
| 企业内网 | Artifactory、安装入口、缺陷与 IM | 负责分发升级与协同回写，不是把 Agent 会话搬上云 |
| 公网 | agent-desk 仓库 | 演示可插拔 Provider 与 Schema，降低内外网分叉 |

## 为何不是「全家桶上 K8s」

本产品定位是**本地 harness**：优先可观测、可中断、可带人工卡点。容器/K8s 更适合集中式平台类项目；此处刻意把运行时压在开发者环境，分发层只负责安装包与技能版本。
