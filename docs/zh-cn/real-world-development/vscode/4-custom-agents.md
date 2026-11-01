---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识编码为可复用智能体，例如执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护统一测试模式的测试智能体。

项目级自定义智能体由 `.github/agents` 中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 的 YAML frontmatter，以及用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体与[智能体技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并告知 AI 如何执行操作。可以这样区分：**自定义智能体**是工作者，**技能**是工具。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本练习中，无障碍自定义智能体会根据无障碍指南审查和更新网站；它还可以调用 PR 工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与 AI 的其他用法一样，需要测试和迭代，找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，由于文字和背景颜色对比度不足，有些用户难以阅读当前深色主题。为解决此无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此希望尽快实现。你将使用自定义智能体生成此功能。

本练习将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目已为无障碍工作创建了自定义智能体。先查看它的内容，了解它如何指导 Copilot。

返回 codespace，打开终端，并从 `main` 创建无障碍功能分支（筛选 PR 将保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带 `name` 和 `description` 的 frontmatter。

3. 继续浏览后续部分，查看其中列出的：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见错误和陷阱。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 目录里的自定义智能体。可以选择智能体，让聊天会话采用它的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会注明。

1. 如果尚未选择，请在 Chat 视图的智能体下拉菜单中选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 在 Chat 视图底部选择智能体下拉菜单（默认显示活动智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改建议并编辑项目文件。每项编辑都会以内联差异的形式显示文件路径和更改内容。
6. 随着编辑进行，**Files changed** 指示器会更新本会话中智能体修改的文件列表。

> [!NOTE]
> 这个过程可能持续几分钟。Copilot 会实际修改存储库，包括 Astro 组件、CSS 和相关测试。

下一练习中，你将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot [自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。自定义智能体可将团队专业知识和标准转化为可复用指导。

你了解了：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体的工作][next-lesson]，在同一会话中审查更改并添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
