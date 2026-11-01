---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
description: "使用 Copilot CLI 模式、自定义项、MCP 工具和拉取请求自动化，构建、验证并交付 Tailspin Toys 的两项更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** 将 GitHub Copilot 作为智能编码助手带入终端。它可以探索代码库、生成代码、运行命令并连接外部工具——所有操作均可在命令行中完成，无需切换到图形化编辑器。

本工作坊围绕 Tailspin Toys 的连续工作流展开：

1. 在 GitHub Codespaces 中准备项目、安装 Copilot CLI 并了解项目。
2. 修改星级评分功能，在浏览器中审查，并手动合并第一个拉取请求 (PR)。
3. 从筛选议题出发，在 Plan 模式中明确方案，在 Autopilot 模式中构建功能，然后在 Interactive 模式中审查。
4. 更新存储库说明，并将其应用于筛选功能。
5. 自定义现有 `quality-checks` 技能并使用它运行项目检查。
6. 添加 Playwright Model Context Protocol (MCP) 服务器，并通过浏览器体验筛选功能。
7. 创建质量保证 (QA) 自定义智能体，审查需求、覆盖率和验证证据。
8. 审查完整的筛选功能更改，并对筛选 PR 使用 Agent Merge。
9. 探索用于管理上下文、模型、共享和可选云委派的实用斜杠命令。

为了让工作坊保持聚焦，你将创建两个 PR：一个用于星级评分，另一个用于筛选功能及其说明更新、技能更新、QA 配置和测试。筛选和质量工作流共用一个对话和分支，因此可以在探索每项工具时继续扩展之前的工作。

## 课程

| 课程 | 主题 | 说明 |
| ------ | ----- | ----------- |
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI，然后了解基本操作 |
| [2. 添加星级评分：快速改进][ex2] | 首次更改 | 显示现有评分和未评分状态，然后合并第一个 PR |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 从议题规划功能，使用 Autopilot 构建，再用 Interactive 模式审查 |
| [4. 使用自定义说明指导 Copilot][ex4] | 上下文 | 探索并更新说明，然后将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 探索现有技能、更改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 在 CLI 中配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 需求和覆盖率 | 创建并选择专用配置，收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查与合并 | 审查完整更改、创建 PR 并使用 Agent Merge |
| [9. GitHub Copilot CLI 斜杠命令][ex9] | CLI 功能 | 探索上下文、模型、共享和可选云委派 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流、可复用的自定义项和更多资源 |
| [可选：集成 Foundry][foundry] | 托管智能体 | 准备模型、部署由目录提供依据的智能体，并将其连接到网站 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 有权创建存储库和 Codespace
- [ ] 熟悉终端或命令行的基本操作

> [!TIP]
> 没有付费计划？经过验证的学生可以通过 [GitHub Education][student-plan] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，请确认管理员已启用 Copilot CLI。

## 开始

**[从先决条件开始 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
