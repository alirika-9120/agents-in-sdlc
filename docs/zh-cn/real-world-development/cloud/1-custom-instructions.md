---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要用特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[说明文件][instruction-files]可以提供这类指导，让 Copilot 不仅了解要做*什么*，还了解应该*如何*完成。

本练习将：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习系列不同，本练习不会比较更新说明前后的提示结果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示说明的影响。稍后的练习中审查云智能体创建的拉取请求时，可以看到说明文件发挥的作用。

## 说明文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应添加注释块，说明文件的作用。

使用说明文件可以向 Copilot 提供正确的信息，让它按照这些实践要求完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定编码约定、首选库，甚至希望添加的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的**每个**请求，都会向 Copilot 发送这个唯一的说明文件。它应包含项目级信息，也就是大多数 Copilot Chat 或 CLI 请求都会用到的上下文，例如技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建说明。可以为 TypeScript 或 Astro 等特定语言提供指导，也可以针对创建 UI 组件或新增单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，说明文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持遵循其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目概述、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用清晰的自然语言，并举例说明代码应该和不应该采用的写法。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过不断尝试，可以找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。本项目中有针对多种任务的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供此功能的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前界面的课程会在相关步骤说明如何使用。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义说明文件

花些时间阅读存储库中现有的说明文件，其中有一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览。其中有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 决定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都符合条件。
5. 查看其中针对本项目单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。将大型说明拆分成更小、可复用的文件，可以让 Copilot 在生成代码时参考具体示例。这里的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 的 **Code formatting requirements** 部分记录了项目编码标准，但尚未规定代码内文档要求。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来只需进行一项编辑：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式记录代码文档。云智能体之后处理分配给它的议题时会读取这项规则。

编辑前，先创建工作分支。云智能体会读取议题目标分支中的说明，不过这里仍会按常规流程提交更改：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。它记录了项目编码标准，但尚未规定代码内文档要求，这正是生成的 helper 缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别创建说明文件（`.github/copilot-instructions.md`）来提供全局指导，也可以创建 `*.instructions.md` 文件来指导特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一课将把议题分配给 Copilot，而它会从 `main` 分支创建新分支。因此，必须先将这次说明更新合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一课中处理议题时，云智能体就会读取这些说明。

> [!TIP]
> 如果希望在本次研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何从项目说明文件中获取上下文，并向 `.github/copilot-instructions.md` 添加了一项新的全库标准。后续练习中，可以在云智能体创建的拉取请求里看到这项标准发挥的作用。

下一步，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更佳自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
