---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本练习系列中，你让 GitHub Copilot 完成了多个任务，以改进用户体验并添加功能：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。接下来检查代码更改，必要时向 Copilot 提供反馈，然后再为团队启用代码审查。

### 场景

正如前面多次强调的那样，采用生成式 AI 并不会改变软件设计和 DevOps 的基本原则。仍需审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队启用代码审查前，先查看 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会异步且自主地执行任务。为确保安全，系统设置了以下限制：

- Copilot 只有对存储库的读取权限，并且**只能**写入供它使用的分支。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- GitHub Actions 工作流必须经人工批准后才能运行。
- 默认情况下，[对外部资源的访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 github.com 的标准 PR 界面查看 GitHub Copilot 云智能体创建的第一个拉取请求：为代码添加文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你也可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 在 github.com 上打开存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待它完成，这可能需要几分钟。等待时可以休息一下，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改会因情况而异。审查时，检查 TSDoc 文档注释、TypeScript 约定和文件注释头；这些要求来自练习 2 中查看的自定义说明文件。
6. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在拉取请求的检查区域排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与团队成员一样，你可以在拉取请求或代码内联评论中提及 Copilot。Copilot 会读取评论，并启动新会话处理反馈。由于结果具有非确定性，无法保证特定输出。可以要求 Copilot：

- 在代码文件开头添加简短注释，说明文件的用途；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用结构。

1. 添加评论并提及 **@copilot**，要求它修改生成的文档。可以使用上述建议，也可以提出其他改进要求。
2. 选择 **View Session**，查看 Copilot 如何开始新会话并进行更改。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。可以继续迭代，直到满意为止。之后可以将 PR 从草稿标记为就绪，再合并到 main 分支。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 在 GitHub.com 上重新打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流。
7. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在拉取请求的检查区域排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已完成相关游戏功能！可以像之前一样继续迭代并请求更改，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释。注意，该议题是在更新自定义说明之前分配的。与之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会据此启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。这个 PR 应包括练习 3 中要求的高对比度模式，以及练习 4 中在 Mission Control 请求的浅色模式。

1. 在 GitHub.com 上重新打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待它完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，尤其要留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在拉取请求的检查区域排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。在 Codespace 中打开终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在重新加载页面后仍然有效。

查看无障碍自定义智能体如何引导 Copilot 按照无障碍最佳实践实现功能。如果发现问题或有改进建议，可以像之前一样在评论中提及 **@copilot**，要求它更新。

## 可选练习 - 继续委派工作

云智能体适合处理待办事项，让你可以把精力放在其他工作上。可以在自己的存储库中创建更多议题并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页创建赞助人兴趣表单；
- 为游戏列表添加分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体练习系列。在这些课程中，你：

- **查看了存储库中的自定义说明文件**，了解它们如何影响云智能体之后的输出；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用自定义智能体**，实现高对比度和浅色模式切换；
- **使用 Copilot agents 页面作为 Mission Control**，监控并引导无障碍工作会话；
- **审查并迭代云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

你已完成云智能体练习系列。还可以继续探索其他练习系列：

- 🖥️ **[VS Code 练习系列](../../vscode/)**：在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习系列](../../cli/)**：从终端交付两项经过审查的更改，并学习 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈配置 `copilot-setup-steps.yml`。

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
