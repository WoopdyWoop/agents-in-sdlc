---
title: "第 4 课 - 使用自定义说明指导 Copilot"
description: "查看存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够访问相关指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。完成筛选功能后，现在可以了解 Copilot 使用了哪些说明、添加一项文档标准，并将其应用于代码。

本课将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应记录其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指导。

通过说明文件，可以确保 Copilot 按照这些实践要求完成任务。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定编码约定、库以及希望包含的注释类型。既可为整个存储库创建说明，也可针对特定文件类型创建说明，以提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：面向该存储库每次请求的单一说明文件，应包含大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

示例项目已包含一组说明文件。先查看现有内容，再进行修改，以了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。这些说明适用于与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro、Drizzle 数据层和测试的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接到其他说明文件和项目现有文件的方式。这样可以将较大的说明集拆成可复用的小文件，并提供可供 Copilot 参考的示例。

## 更新说明以符合团队指导

现有文件已经打下基础。现在修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认其准确说明代码。

## 总结和后续步骤

你已了解 Copilot 如何读取本项目的说明，并应用了一项新的项目标准。具体来说，你：

- 查看了存储库说明和按路径限定范围的说明文件。
- 更新说明文件，要求遵循编码标准。
- 查看了说明文件对生成代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
