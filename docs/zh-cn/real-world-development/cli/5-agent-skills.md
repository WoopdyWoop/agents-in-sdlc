---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。我们已经手动验证代码，也使用说明文件确保代码符合标准。接下来还要处理测试、lint 和持续集成 (CI) 中的其他工作。

这类任务适合使用**智能体技能**。技能可以指导 Copilot 正确执行这些操作。

本课将：

- 探索现有的 `quality-checks` 技能。
- 自定义技能结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，由智能体按需加载。技能本质上是一个以技能名称命名的文件夹，其中包含 `SKILL.md`。该 Markdown 文件包含定义技能名称和描述的 frontmatter、技能用途概览，以及何时调用技能的指导。文件夹也可以包含供技能调用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或子目录。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，成为团队共享和复用的存储库内容；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

Tailspin Toys 团队创建了名为 `quality-checks` 的技能，用于运行单元测试、lint 和类型检查。现在查看其内容。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。description 会帮助 Copilot 判断何时调用技能。
3. 阅读说明，了解它如何指导 Copilot 执行测试和 lint。
4. 注意当前技能中还没有 **Results output formatting** 部分。

## 修改之前运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言。现在请 Copilot 运行该技能中的三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

报告还应说明执行了哪些操作、是否成功以及工具报告的内容。现在更新技能，生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改完成后，查看更新后的技能如何工作。Copilot CLI 可以在不重启对话的情况下重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 发送与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看报告并与第一次运行的结果进行比较。

## 总结和后续步骤

你已自定义并使用现有的智能体技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的报告格式。
- 重新加载并运行了技能。

这项更改会随筛选功能一起包含在功能 PR 中。接下来，[通过 Playwright MCP 服务器][next-lesson]让 Copilot 直接操作网站。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和配套导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
