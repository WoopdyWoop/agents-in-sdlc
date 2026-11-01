---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在整个实验中，你使用 GitHub Copilot 执行了多项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，进一步改进结果。

### 场景

加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码并遵循常规 DevOps 流程。接下来审查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提供的建议，然后再向团队开放审查。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体会异步、无人监督地执行任务，因此设置了一些安全限制：

- Copilot 仅对存储库具有读取权限，只能写入它使用的分支。
- 云智能体在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 的常规 PR 界面，查看 GitHub Copilot 云智能体创建的第一个 PR——代码文档更新。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似于 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加到代码中的 TSDoc 文档注释和其他文档。具体更改会有所不同。注意 TSDoc 文档注释、TypeScript 约定和文件注释头，它们来自练习 2 开始时查看的自定义说明文件。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有待审批工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 检查区域的队列中。正常情况下，单个 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

你也可以在 PR 评论或代码内联评论中提及 Copilot，让它更新工作。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法给出固定提示文本；可以考虑要求 Copilot：

- 在每个代码文件顶部添加简要说明其用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 在生成的文档 PR 中发表评论，并像提及用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档相关改进。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并更新内容。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request 按钮](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿状态改为准备审查，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为游戏详情页添加相关游戏功能而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有待审批工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在 PR 检查区域的队列中。正常情况下，单个 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。你可以像之前一样继续迭代，例如调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时还未更新自定义说明）。在 **Conversation** 选项卡中发表评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查由无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 里提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，特别关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有待审批工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 检查区域的队列中。正常情况下，单个 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。
10. **可选：**在 Codespace 中切换到 Copilot 创建的分支，以手动测试无障碍功能：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在页面重新加载后仍然保留。

无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可像之前一样在评论中提及 **@copilot** 并请求更新。

## 可选练习 - 继续委派工作

云智能体适合处理真实待办事项，让你可以专注于其他工作。可以为存储库创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体练习。在整个系列中，你：

- **检查了存储库附带的自定义说明文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为 mission control**，监控并引导进行中的无障碍会话。
- **审查并迭代优化云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并审批工作流。

## 复习和后续步骤

云智能体练习已完成。其他练习可以补充本系列所学内容：

- 🖥️ **[VS Code 练习](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习](../../cli/)**：通过终端交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
