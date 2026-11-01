---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)可让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，云智能体会在后台接手任务、探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

本系列练习将添加云智能体会遵循的自定义说明，然后分配 GitHub 议题并让它实现功能。你将审查并使用自定义智能体来指导其工作，在智能体仪表板中监控和引导会话，最后审查它创建的拉取请求并迭代优化结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云智能体将遵循的自定义说明 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 查看并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代优化][ex5] | 审查 | 审查 PR、迭代优化 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确认你具备：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？经过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可完成所有练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用云智能体；Copilot Free 除外。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用该功能。

> [!NOTE]
> 本练习不涉及 MCP。若想了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 练习][cli-harness]或 [VS Code 练习][vscode-harness]。

[cli-harness]: ../cli/
[vscode-harness]: ../vscode/

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
