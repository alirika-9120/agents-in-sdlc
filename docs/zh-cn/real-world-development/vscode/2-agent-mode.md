---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发人员往往需要先找到所有相关文件，再进行修改、运行测试、调试、查找遗漏的文件并继续更新，工作流程很容易反复循环。

Copilot 代理模式可以帮助完成这些工作。

代理模式可以在 IDE 中更自主地工作。它会像开发人员一样探索项目结构、修改相关内容、运行测试等任务，并自动修复发现的问题。接下来将使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 代理模式如何探索项目、查找相关文件并协调多文件更改；
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- 合并前如何审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一课已经添加了 publishers helper，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先查看 Tailspin Toys 网站当前的功能，再进行修改。

该网站是一个以开发者为主题的棋盘游戏众筹平台，可列出游戏并显示详细信息。网站是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面在 frontmatter 中通过 Drizzle ORM 直接查询本地 SQLite 数据库；没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一课结束后，你应该已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前 codespace 的私有通道，网站正在其中运行。

### 浏览网站

网站启动后，可以浏览其主要功能：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开包含更多信息的详情页，例如标题、说明、发行商和类别。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新对话。不同练习所用的模式和模型可能不同，相关步骤会注明要求。

从模板创建项目时，系统已自动创建一组议题作为待办事项。让 GitHub Copilot 帮你查看这些事项。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此步骤中包含该模型，希望由此获得相对一致的体验。不过，大语言模型具有概率性，收到的建议可能与示例不同，这是正常现象。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会调用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问 GitHub 存储库中的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的事项为列表排序。

## 查看说明文件

开始生成代码前，先查看将用于向 Copilot 提供上下文的说明文件。这里会使用 [UI 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了为网站添加功能时应采用的做法。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中关于如何添加功能的整体指导，包括：
   - 应用架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向 Astro、Tailwind CSS 和 Drizzle 等特定文件类型说明的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分复杂指导，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用做三项修改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 代理模式可以执行这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 在 Chat 视图的智能体下拉菜单中确认已选择 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 让 Copilot 根据待办事项中的相关议题实现功能，并发送以下提示：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层 helper、UI 和测试。随后它会修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，编辑器中可能会出现 **Keep** 和 **Undo** 按钮。工作结束后，所有更改都会显示这两个选项；在处理中无需提前选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认是否运行测试的屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停，测试也可能暂时失败。这是正常的。Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整聊天会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能：

- 使用代理模式跨 UI 和数据层实现新功能；
- 让 Copilot 探索项目、找到相关文件并协调更改；
- 合并前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索练习 - 实现分页

随着游戏列表增长，网站需要支持分页。使用本练习学到的方法，让 Copilot 实现分页。可以考虑以下事项：

- 遵循现有最佳实践，包括使用已有说明文件；
- 决定分页方式，例如是否允许用户选择每页大小，还是固定大小；
- 在提示中提供 Copilot 所需的指导；
- 根据需要迭代，要求 Copilot 修改并补充上下文。这是使用 Copilot 的正常工作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
