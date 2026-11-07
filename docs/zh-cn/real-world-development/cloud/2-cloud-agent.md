---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或长期搁置的功能请求。GitHub Copilot 云智能体可以自主完成更新代码和添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本课将了解如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有为项目编写文档，因此首先需要为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望在每款游戏的详情页显示同类别的其他游戏，让用户可以继续浏览。当前不需要完善的实现，只需要一个可用于用户体验验收测试的版本。虽然这项需求目前受阻，但还有优先级更高的议题。

这两项任务都可能被推迟，很适合交给 Copilot 云智能体异步处理。开发人员可以继续处理其他工作，之后再回来审查 Copilot 的成果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式与它协作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的说明

GitHub Copilot 没有魔法，使用它并不能跳过思考。即使看似简单的任务也可能有复杂的细节，因此要注意[为 Copilot 云智能体明确任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程助手：分阶段工作、学习、尝试并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其作用。如果你已完成自定义说明练习，就已经看过 `.github/copilot-instructions.md`；否则，现在可以快速查看。分配任务前，先只读查看存储库中的说明文件，之后便能了解它们对云智能体工作的影响。

在 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体。审查最终 PR 时，请留意 TSDoc 文档注释、TypeScript 约定和文件注释头；这些都来自上述说明文件。后续练习会再次检查这些内容。

## 为 Copilot 云智能体配置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本，确保环境就绪。将任务分配给 Copilot 时也一样，它会像软件工程师一样执行工作。

云智能体在 [GitHub Actions][github-actions] 中运行。可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，让工作流在智能体开始工作前运行，以安装所需工具和依赖项。项目已预先配置好此工作流，以支持练习流程。它会准备 Node.js、项目依赖、端到端测试所需的浏览器，以及单个 Astro 应用所用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 Actions 选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 只授予步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在是 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，供构建和测试使用。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 它使用 `workflow_dispatch` 触发器，因此可以在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试设置是否正常，无需等待 Copilot 启动它。

## 添加文档

大多数项目都存在文档过时或完全缺失的问题。这类技术债务经常被搁置，降低效率，也让维护代码库或引入新开发人员变得更加困难。Copilot 擅长生成文档，因此很适合将这项议题分配给云智能体。它会在后台生成所需文档，后续练习再审查结果。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题可能类似 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表和 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，查看 Copilot 如何探索代码、了解当前状态、暂停思考并制定方案，然后创建代码。

这可能需要几分钟。云智能体的主要优势之一是可以异步执行任务，让开发人员继续处理其他事情。接下来利用这一点，将另一个任务分配给云智能体，同时开始为应用添加功能。

## 为游戏详情页添加相关游戏

Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组任务。设计团队需要的相关游戏功能可能并不复杂，但仍可将这段时间用于其他工作。现在把它分配给云智能体。

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

不久后，议题的第一条评论中会出现 👀，表示 Copilot 已开始工作。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二个请求。云智能体的工作方式类似软件工程师，因此无需一直主动监控；等它完成后再审查即可。现在可以开始创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给它，让它异步处理技术债务、开发新功能，或协助将代码迁移到其他框架。

本课介绍了以下内容：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

云智能体正在后台处理任务。接下来创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并能读取前面课程中介绍的自定义说明。

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
