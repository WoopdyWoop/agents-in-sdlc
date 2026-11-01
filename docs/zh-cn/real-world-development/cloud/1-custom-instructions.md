---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应该了解背景信息，就需要确保它能访问这些上下文。[说明文件][instruction-files]用于提供此类指导，帮助 Copilot 理解不仅要做*什么*，还要*如何*完成。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，本练习不会运行前后提示进行比较。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示说明的影响。稍后审查它创建的拉取请求时，可以看到说明文件的作用。

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
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

示例项目展示了一种管理说明文件的代表性做法：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 针对文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码应如何编写或避免如何编写的示例。

说明文件没有唯一正确的组织方式。通过实验找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览本项目时，可以看到针对多种任务的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供该功能的方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相应课程会在相关处说明。
>
> 如需模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

查看此存储库附带的核心 `copilot-instructions.md` 和其他 `*.instructions.md` 文件。可在 GitHub Web UI 或 codespace 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下查看嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹，查看其中针对 Astro、Drizzle 数据层和测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段定义相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它对其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。说明文件中的路径相对于该说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来仅需进行一项编辑：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中编写文档。之后的练习中，云智能体会在处理议题时读取该规则。

编辑前先创建分支。云智能体会从议题目标分支读取说明，但仍应规范提交这些更改：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于提供项目级信息。现在添加一项存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。注意现有标准尚未要求代码内文档，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件既可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习中，分配给 Copilot 的议题会从 `main` 分支开始工作，因此说明更新必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后的练习中，云智能体会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上工作，也可以跳过分支步骤并直接提交到 `main`。此处采用分支是为了模拟真实项目的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，云智能体创建的拉取请求会体现该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
