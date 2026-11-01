---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你已请 Copilot 云智能体处理三项改善用户体验和添加功能的任务。云智能体会异步自主工作，但监控这些任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，可以查看所有具有未关闭拉取请求 (PR) 的智能体任务、查看它们执行的操作，并引导正在进行的会话。

本课将：

- 使用智能体页面监控云智能体任务。
- 引导进行中的会话，要求添加其他功能。

## 场景

将任务分配给智能体以实现高对比度模式后，团队意识到也应添加浅色模式。由于网站样式和切换功能已在更新，适合同时添加此功能。现在引导智能体，使其同时实现高对比度模式和浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 所列步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 注意右侧显示的 PR 面板。可以通过它查看 PR 和已更改文件，进一步监控工作。

## 引导云智能体

现在向正在实现高对比度模式的 Copilot 请求，同时添加浅色模式。

1. 选择与添加高对比度模式有关的会话。确切标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时应会看到类似的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示的引导活动会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示并将其加入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务。在发送提示时，它可能正在执行某个步骤；完成该步骤后会将新请求加入工作流程。

这可能还需要几分钟，可以趁此机会回顾目前学到和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。可以跟踪所有进行中和已完成的任务、查看正在执行的工作，并调整进行中任务的范围或提供补充指导。

本课介绍了：

- 使用 mission control 和智能体页面监控云智能体任务。
- 引导进行中的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来[迭代优化它创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
