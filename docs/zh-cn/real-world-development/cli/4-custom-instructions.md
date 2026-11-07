---
title: "练习 4 - 使用自定义说明引导 Copilot"
description: "查看存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要用特定方式完成，就要确保 Copilot 能访问相关指导。[说明文件][instruction-files]不仅描述想要*什么*代码，还描述代码应当*如何*组织。完成筛选功能后，现在可以查看 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课将：

- 了解存储库说明和按路径限定范围的说明文件如何提供给智能体；
- 更新说明文件，确保遵循编码标准；
- 观察说明文件对代码的影响。

## 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（包括适用时注入的 `db` 参数）；
- 可复用的 Astro 组件应说明 `Props` 接口约定，相关代码更改时也应更新注释；
- 保留现有格式和 lint 指南。

使用说明文件可以向 Copilot 提供正确的信息，让它按照这些实践要求完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定编码约定、首选库，甚至希望添加的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的**每个**请求，都会向 Copilot 发送这个唯一的说明文件。它应包含大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持范围会因环境而异。依赖某种格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看本项目中的自定义说明文件

起始项目中已包含一组说明文件。先查看现有内容，再进行更改，以便了解更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段，它使用 glob 决定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明拆分成更小、可复用的文件，并为 Copilot 提供可参考的示例。

## 更新说明文件以符合团队指导

现有文件已经提供了良好的起点，但仍有一项遗漏。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 在该部分末尾添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，以便在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的分支为 `game-filters-cli`。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看所有更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何从说明文件获取上下文，并将一项新标准应用到筛选功能代码。本课中，你：

- 了解存储库说明和按路径限定范围的说明文件如何提供给智能体；
- 更新说明文件，确保遵循编码标准；
- 观察了说明文件对代码的影响。

下一步，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
