---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到相关文件、进行修改、运行测试、调试、找出遗漏的文件并再次修改，工作往往会不断重复。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式能够在 IDE 中更自主地工作。它会先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。下面了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将更改合并到代码库之前，审查智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习已添加 publishers helper，现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站之前，先了解当前功能。

该网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面在 frontmatter 中直接通过 Drizzle ORM 查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可在 GitHub Codespace 中按以下步骤运行：

1. 返回 codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 网站服务器的专用隧道。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后可打开详情页，查看标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会注明。

网站初始版本已具备基本功能，现在可以添加新能力。先查看待办事项。模板会自动创建一组议题，请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，以便尽量保持建议的一致性。由于大型语言模型具有概率性，实际建议可能不同，这是正常且符合预期的。

> [!NOTE]
> Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示，请 Copilot 查看议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还会根据它认为对用户最有用的程度排列议题优先级。

## 查看说明文件

生成代码之前，先查看将用于向 Copilot 提供上下文的说明文件。项目中的 [用户界面 (UI) 说明][ui-instructions]会指导如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 项目架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 针对 Astro、Tailwind CSS 和 Drizzle 等文件类型的说明文件链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分大型说明集，并提供示例。

## 实现筛选功能

完成筛选至少需要对应用做三项修改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

这些测试必须在合并前运行并通过。Copilot 智能体模式可以完成上述工作。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关数据层、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，具体过程可能持续几分钟。期间编辑器中可能出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示这两个选项；工作尚在进行时无需选择。

7. Copilot 提示时选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。Copilot 会在代码生成和测试之间反复迭代，直到任务完成且未检测到错误。

   ![Copilot 智能体模式完整聊天会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 添加功能。你了解了如何：

- 使用智能体模式跨 UI 和数据层实现新功能。
- 让智能体模式探索项目、识别相关文件并协调更改。
- 在合并之前审查智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索练习 - 实现分页

随着游戏列表增长，网站需要分页。使用本练习学到的技能，请 Copilot 实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 决定分页方式，例如是否允许用户选择页面大小，还是使用固定值。
- 编写提示时提供所需指导，以便 Copilot 按预期实现功能。
- 可能需要通过追加提示和上下文多次迭代。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
