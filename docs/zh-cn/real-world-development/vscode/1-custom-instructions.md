---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要用特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]指导代码生成。

本练习将：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot；
- 在当前说明生效时生成筛选功能的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准；
- 重新运行相同提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能基础提交到 `main`，供下一课中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 具有非确定性。本练习的目的是观察更新说明后行为变化的*趋势*，而不是要求输出逐字符匹配。

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
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。查看本项目中的文件时，可以看到其中有针对多种任务的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
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
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 决定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都符合条件。
5. 查看其中针对本项目单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。将大型说明拆分成更小、可复用的文件，可以让 Copilot 在生成代码时参考具体示例。这里的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 的 **Code formatting requirements** 部分记录了项目编码标准，但尚未规定代码内文档要求。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一课使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新对话。每个练习会在相关步骤说明应使用的模式和模型。

先在当前说明生效的情况下发送提示，以便观察自定义说明的影响。稍后会更新说明文件并重新发送相同提示。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪个位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以修改文件。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改 helper 文件及其测试。
6. 注意，生成的 helper 是一个带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；本练习要观察的是更新说明后行为的*一致性*改善。

## 添加新的存储库标准

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

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话上下文。
2. 在 `src/lib/publishers.ts` 中打开文件，让 Copilot 聚焦于正确的位置。
3. 重新发送之前的**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在生成的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的 publisher 数据访问辅助函数。
    * 提供从数据库检索 publisher 信息的函数。
    */
   ```

5. 注意，生成的函数现在会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有 publisher 的列表，包含其 id 和 name。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为 publisher 对象数组。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到实际代码中。下一课会在此基础上继续构建。

## 提交说明并推送分支

说明文件和其他项目资源一样，都通过源代码管理进行维护。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认说明更新和 helper 更改已存在：

   ```bash
   git status
   ```

3. 暂存说明更新，以及 Copilot 为 helper 基础更改的所有文件，然后提交：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并将其关联到待办事项中的文档标准议题。Copilot 可以使用项目已连接的 GitHub 工具代为查找议题、创建用于关闭该议题的 PR，并合并它。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已经配置好 GitHub 工具，无需额外设置。Copilot 首次使用时，VS Code 可能会提示你登录 GitHub。按照提示完成授权。后续练习会介绍这种连接所用的模型上下文协议。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选中 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中包括说明更新和为后续筛选工作准备的 publishers helper：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 当 Copilot 提示允许它代表你执行 GitHub 操作时，先审查操作内容，再酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示允许它执行 GitHub 操作和 shell 命令时，先审查操作内容，再酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结与后续步骤

你了解了 Copilot 如何从项目说明文件中获取上下文，并使用 VS Code 中的 Copilot Chat 完成了以下工作：

- 发送代码生成提示，观察 Copilot 如何遵循*现有*说明；
- 向 `.github/copilot-instructions.md` 添加新的全库标准；
- 重新发送相同提示，观察生成的代码如何采用新标准；
- 将说明更新和 helper 基础提交到 `main`，供下一课继续使用。

下一步，Copilot 会在[代理模式][next-lesson]下使用这些说明，跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更佳自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
