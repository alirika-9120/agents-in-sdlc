---
title: "练习 2 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发浏览器中检查更改，并合并第一个拉取请求。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，接下来为项目做第一个更改。先从小处着手：游戏数据中已有星级评分，但主页的游戏卡片尚未显示评分。请智能体显示该评分，审查更改，并将它作为第一个拉取请求合并。

本课将：

- 在功能分支中开始专注于一项任务的 Copilot 对话；
- 请智能体对项目进行小幅更改；
- 使用 `/diff` 审查更改；
- 在转发浏览器中运行应用并确认效果；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示评分。不过，主页上的游戏卡片只显示标题、类别、发行商和说明。先让智能体在每张卡片上显示现有评分，作为第一次会话的小型独立任务。

## 对话的组成

**对话**是你在 Copilot CLI 中处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

开始新对话，实现此功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确认启动新会话。
5. 发送以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、找到用于显示游戏详情的文件，并创建所需代码。现在已使用 Copilot CLI 添加了一项功能。

## 审查差异

合并前应审查所有 AI 生成的更改，即使它们很小。现在直接在 Copilot CLI 中查看更改。

1. 输入 `/diff`，检查每个已更改的文件。
2. 确认游戏卡片在有评分时显示数值；当 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此生成的代码可能不同。审查实际行为，不要期待每次都得到完全相同的实现。

## 检查更改

不能只读代码并假设它能正常运行。请 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的 UI。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，请它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已创建，现在为更改创建拉取请求 (PR)，将代码合并到项目中。

1. 请默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会创建 PR、推送现有提交并显示 PR URL。
3. 在 Copilot CLI 显示的 URL 上按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux）并选择链接，打开 PR。
4. 审查已更改的文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request** 并确认。
6. 返回 Codespace，在 Copilot CLI 中输入 `/exit` 退出。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜你使用 GitHub Copilot CLI 交付了第一个更改。本课中，你：

- 在功能分支中开始了专注于单项任务的 Copilot 对话；
- 引导智能体小幅修改游戏卡片；
- 使用 `/diff` 审查更改；
- 运行应用，并在转发的浏览器中确认星级评分；
- 创建并合并了第一个拉取请求。

接下来，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
