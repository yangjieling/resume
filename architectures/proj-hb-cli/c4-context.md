# System Context — AI Coding Agent 编排工作台

展示本系统与使用者、外部 Coding Agent、缺陷/通知/分发系统的边界。

```mermaid
flowchart TB
  subgraph People["人员"]
    Eng["研发工程师<br/>发起任务 / 审批卡点"]
    Pilot["试点团队管理员<br/>安装升级 / 分发技能"]
  end

  Desk["AI Coding Agent 编排工作台<br/>hb-cli / agent-desk<br/>本地任务编排 · 技能 · 人工卡点"]

  subgraph Agents["外部 Coding Agent"]
    Joy["JoyCode"]
    Claude["Claude Code"]
    Codex["Codex"]
    Cursor["Cursor Agent"]
  end

  subgraph Platforms["协作与平台"]
    Issue["缺陷 / 任务平台"]
    Notify["飞书 / 钉钉 / Webhook"]
    Artif["Artifactory / 内网分发"]
    GitHub["GitHub<br/>agent-desk 开源仓"]
  end

  Eng -->|使用 CLI / 本地面板| Desk
  Pilot -->|安装包 · 技能注册| Desk
  Desk -->|启动子进程执行| Joy
  Desk -->|启动子进程执行| Claude
  Desk -->|启动子进程执行| Codex
  Desk -->|启动子进程执行| Cursor
  Desk -->|拉取 / 回写缺陷| Issue
  Desk -->|卡点深链通知| Notify
  Notify -->|继续 / 中止回写| Desk
  Artif -.->|内网安装与升级| Desk
  Desk -.->|同源开源演示| GitHub
```

## 要点

- **系统在本地**：控制面与执行器跑在工程师机器上，避免把仓库与密钥默认送上远端黑盒。
- **Agent 是外部可替换单元**：统一 Provider 收口差异，而不是绑死某一家 CLI。
- **人在环路**：关键节点经通知深链回到工作台，形成「Agent 提议、人来批准」。
