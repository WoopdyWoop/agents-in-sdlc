---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或长期留在待办事项中的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成任务后，它会创建草稿拉取请求，供开发者审查。这样可以卸载繁琐任务、加快开发流程，让开发者专注于更重要的工作。

本课将使用 Copilot 云智能体：

- 自定义代码生成环境。
- 了解如何安全地执行操作。
- 学习清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有为代码编写文档。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页显示相关游戏（同一类别中的其他游戏），让用户可以继续浏览。目前不要求实现经过打磨的版本，只要能够用于 UX 验收测试即可。虽然这项工作目前有所阻碍，但优先级低于其他议题。

这两项任务都可能被不断延期，适合交给 Copilot 云智能体异步处理。开发者可以先处理其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法，使用它并不能省去思考。即使看似简单的操作，深入分析后也可能很复杂，因此应谨慎[界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 配对程序员：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看。分配任务之前，先以只读方式查看存储库中的说明文件，之后便能识别它们的影响。

可在 GitHub Web UI 或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些说明如何限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，注意查看其拉取请求中的 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述说明文件；稍后审查 PR 时还会再次提及。

## 为 Copilot 云智能体设置开发环境

编写代码通常需要特定环境和初始化脚本，以确保一切就绪。将任务分配给 Copilot 时也一样，因为它会像软件工程师一样工作。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[特殊设置工作流][setup-workflow]自定义其环境，在开始工作前安装所需开发工具和依赖项。项目已提前配置此工作流，以支持练习流程。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需浏览器，以及迁移和填充后的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设为步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 其中的 `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试设置是否成功，而不用等待 Copilot 运行它。

## 添加文档

大多数人都理解文档的重要性，但许多项目的信息已经过时或完全缺失。这类技术债务常被搁置，会降低工作效率，也让维护代码库或引入新团队成员变得更加困难。Copilot 擅长编写文档，因此适合把这项工作分配给云智能体。它会在后台生成所需文档，后续练习中再审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项。它正在创建环境（运行前面介绍的工作流）、分析项目并制定解决问题的方法。

13. 查看 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面，才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解状态、暂停思考并制定计划，以及创建代码。

该过程可能需要几分钟。Copilot 云智能体的一个主要目标是异步执行任务，让你可以专注于其他工作。接下来会再分配一项任务，然后转而为应用编写其他功能。

## 在游戏详情页添加相关游戏

将工作拆分给 Copilot 云智能体后，你可以专注于一组任务，而它处理另一组任务。为设计团队添加相关游戏可能不会花太久，但仍会占用你处理其他工作的时间。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二项请求。它会像软件工程师一样工作，无需一直监控；完成后再回来审查即可。现在可以转而创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、新功能或框架迁移等任务。

本课介绍了：

- 自定义代码生成环境。
- 安全地执行操作。
- 清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，接下来可以了解如何创建和使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
