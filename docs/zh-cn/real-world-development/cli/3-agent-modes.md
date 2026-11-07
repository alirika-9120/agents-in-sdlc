---
title: "练习 3 - 智能体模式：Plan 和 Autopilot"
description: "先用 Plan 确定实现方案，再用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。更大的更改需要更全面的流程。GitHub Copilot CLI 可以配合组织现有的开发流程，帮助你按正确的方式构建所需功能。本课开始了一组使用智能体进行开发的练习：从议题出发构建新功能、确认代码有效且功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能练习会继续使用同一对话和分支。实际项目中通常会为不同类型的文件创建不同分支或 PR；此处采用简化流程，以便聚焦核心概念。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中确定需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发浏览器中手动验证功能。

后续练习会继续开发该功能：更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩展，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合多个类别等细节需要在编码前确定。你将使用 Plan 模式明确这些决定，然后让 Autopilot 在限定范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体不会改变软件开发的基本原则，反而让这些原则更加重要。开发人员通常会：

1. 从描述需求的已登记议题开始；
2. 规划需要构建的内容；
3. 编写并审查代码；
4. 运行测试以验证代码；
5. 手动验证新功能；
6. 创建拉取请求 (PR)；
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述做法类似。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过验证。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你确认后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直到认为任务完成。

先在 Plan 模式中审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选择标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，并返回 **Session** 选项卡。

现在提示中会出现 `#7` 或类似编号。使用 `#` 可将 GitHub 议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

先规划功能或任务的实现方式，可以在交由 Copilot 执行前明确要求。复杂任务尤其适合先花些时间规划。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式，确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方式并制定计划。

3. Copilot 可能会询问筛选功能应如何运行。按照自己的偏好回答即可，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划与自己的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划制定并审查完成后，可以开始实现。使用 Autopilot 让 Copilot 按计划迭代，直到认为工作完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，应先解决环境问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，和其他代码一样，需要先审查再合并。现在审查代码并运行网站，确认效果符合预期。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式，确认指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，审查筛选功能实现和测试。
3. 对照议题和规划时确定的要求检查结果。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果；如果有失败项，请让 Copilot 修复。

## 体验新功能

代码看起来没问题，但还需要确认能否运行。像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置筛选和无匹配结果的行为符合议题及规划中的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你使用不同的对话模式构建并审查了一项功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中确定需求；
- 使用 Autopilot 模式实现新功能；
- 审查了代码；
- 在转发浏览器中手动验证了功能。

下一步，[使用自定义说明][next-lesson]深入了解代码生成，确保生成的代码遵循书面标准。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
