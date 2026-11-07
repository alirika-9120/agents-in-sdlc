---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "查看或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中检查筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码只是开发工作的一部分。我们还需要处理数据和外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以扩展 Copilot CLI 内置的能力，为它提供更多工具和服务。

本课将：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚不可用，则添加 Playwright MCP 服务器；
- 让智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更改，还需要实际与界面交互。我们希望让 Copilot 像用户一样操作网站，进一步自动化更改检查，并提高对功能正确性的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。通过 MCP，智能体可以实时获取最新信息，并代表你执行操作。

MCP 服务器充当智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都会提供一组智能体可以使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，方便用户查找和贡献 MCP 项目。

> [!CAUTION]
> 应像对待项目其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入服务器名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 确认服务器类型为 **STDIO**，按 <kbd>Enter</kbd>，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 让 Copilot 探索功能

你已手动确认功能符合预期。现在使用 Copilot 中可用的 Playwright MCP 服务器完成同样的检查。

1. 发送以下提示，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确指定 MCP 服务器，Copilot 会自行选择合适的工具。不过，指出需要使用的服务器有助于保持结果一致，也能减少一些提示内容。

2. 观察 Copilot 在浏览器中执行的步骤，确认功能是否正常。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结与后续步骤

恭喜你使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中探索了功能。本课中，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果 Playwright MCP 服务器尚未配置，则添加了该服务器；
- 让智能体操作浏览器并探索筛选功能。

下一步，创建[自定义 QA 智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么？为何大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
