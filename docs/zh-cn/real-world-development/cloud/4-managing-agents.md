---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你已让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体可以异步自主工作，但仍需要监控这些任务。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 Mission Control 中，可以查看所有已创建开放拉取请求的智能体任务，检查智能体执行的操作，并引导正在进行的会话。

本练习将：

- 查看智能体页面，监控云智能体任务；
- 引导正在进行的会话，请求添加其他功能。

## 场景

在分配高对比度模式任务后，团队意识到也可以同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，将这项功能一并完成很合理。现在引导智能体，确保它同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的任务当前状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表应包含已分配给 Copilot 的任务，例如：
   - 更新代码文档；
   - 生成用于修改游戏的 API；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，例如：
   - 从存储库检出代码；
   - 为 Copilot 创建环境；
   - 设置 MCP 服务器；
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 当前的工作状态和执行方式而变化。

4. 还可以查看右侧的拉取请求面板，监控 PR 和已更改的文件。

## 引导云智能体

现在已了解如何查看正在处理的任务，接下来要求 Copilot 在实现高对比度模式的同时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。确切标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！收到消息时，它可能正在执行某个步骤，会在完成该步骤后把新要求加入工作流程。

与之前一样，完成工作可能需要几分钟。此时可以暂停一下，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，它是监控并引导 GitHub Copilot 云智能体任务的中心位置。在 Mission Control 中，可以跟踪所有正在进行和已完成的任务、查看工作情况，并在会话进行时调整范围或补充指导。

本课介绍了以下内容：

- 使用 Mission Control 和 agents 页面监控云智能体任务；
- 引导正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
