---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot Chat 在 VS Code 中实时执行这些操作，你可以持续查看每次工具调用和文件编辑。可以内联审查差异、接受或拒绝单项更改，并通过后续提示引导对话以完善或扩展工作，无需离开编辑器。

本练习将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，添加浅色模式切换。

## 场景

上一练习中使用的无障碍自定义智能体正在实现高对比度模式。工作期间，设计团队提出了额外需求：为觉得默认主题过暗的用户添加浅色模式切换。不必从头启动新对话，可以引导现有会话扩展功能。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每次文件编辑都会以内联差异和 **Keep** / **Undo** 控件显示。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑以及需要运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本会话修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式。完成后应会显示更改摘要。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中继续添加浅色模式。由于会话上下文未丢失，智能体可以使用之前的工作信息，包括涉及的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会继续工作，探索现有更改并提出浅色模式的修改。
3. 按照高对比度模式的审查方式，逐一检查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体方向不符合预期（例如尝试重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止，然后发送澄清提示。

> [!TIP]
> 引导进行中的会话比从头开始更快。可以用后续提示要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要新增第三个切换项，把浅色模式放进现有设置面板”）或扩展范围（“为两个切换项添加键盘快捷键”）。

## 审查工作集

提交之前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开其中几个文件，查看差异。确认包含自定义智能体规定的无障碍模式，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导了一个进行中的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作而不丢失上下文。

本课介绍了：

- 实时监控智能体工作。
- 接受或拒绝单个文件编辑。
- 在会话进行期间通过后续提示引导智能体。

在[下一课][next-lesson]中，你将发布无障碍更改并审查本实验中本地自定义智能体生成的工作。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
