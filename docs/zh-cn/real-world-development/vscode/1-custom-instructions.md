---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应该了解背景信息，就需要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是[说明文件][instruction-files]，它们用于指导代码生成。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在现有说明下生成用于筛选功能的第一块数据切片（publisher helper）。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。
- 再次运行相同提示，观察生成代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能与部分标准不同。Copilot 具有概率性。此练习旨在观察更新说明后行为变化的*趋势*，而不是要求输出逐字符一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件顶部都应有注释块，说明该文件的作用。

使用说明文件，可以确保 Copilot 根据这些实践要求完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定编码约定、库以及希望包含的注释类型。既可为整个存储库创建说明，也可针对特定文件类型创建说明，以提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：面向该存储库每次请求的单一说明文件，应包含大多数聊天或 CLI 请求都会用到的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如为 TypeScript、Astro、UI 组件或单元测试提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

完整讨论说明文件的创建方式超出本工作坊范围，但示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 针对文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码应如何编写或避免如何编写的示例。

说明文件没有唯一正确的组织方式，就像使用 AI 也没有唯一正确的方法。通过实验找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览本项目时，可以看到针对多种任务的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供该功能的方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前环境的课程会在相关处说明。
>
> 如需模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

查看此存储库附带的核心 `copilot-instructions.md` 和其他 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹，查看其中针对 Astro、Drizzle 数据层和测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段定义相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它对其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。说明文件中的路径相对于该说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

1. 返回之前使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*先使用 Copilot Chat

为了观察自定义说明的影响，先在当前说明下发送提示。更新说明后，再次发送相同提示以比较结果。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异，相关步骤会注明。

1. 关闭之前练习打开的编辑器标签页，让 Copilot 仅获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放置的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改 helper 文件及其测试。
6. 注意生成的 helper 是类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的 publisher 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，有时即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；观察说明更新带来的*一致性*改进仍然是重点。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。注意它记录了项目编码标准，但尚未要求添加代码文档，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在也会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中，下一练习会继续使用这些代码。

## 提交并推送说明和分支

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和 helper 更改均已存在：

   ```bash
   git status
   ```

3. 暂存说明更新和 Copilot 为 helper 修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持性更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联到待办事项中的文档标准议题。Copilot 可使用已经连接到项目的 GitHub 工具查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已连接 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。后续练习会介绍这种连接所用的 Model Context Protocol。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明说明更新和新的 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 当 Copilot 提示允许其代表你执行 GitHub 操作时，请先审查命令，再选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示允许执行 GitHub 操作和 shell 命令时，请先审查命令，再选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的说明，并使用 VS Code 中的 Copilot Chat 完成了以下事项：

- 使用现有说明发送代码生成提示，并观察 Copilot 的输出。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将说明更新和 helper 基础提交到 `main`，供下一练习使用。

接下来，Copilot 会在[智能体模式][next-lesson]中应用这些说明，在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
