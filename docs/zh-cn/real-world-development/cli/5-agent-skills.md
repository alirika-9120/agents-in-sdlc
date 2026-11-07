---
title: "练习 5 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能，自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。此前已手动验证代码，也使用说明文件确保遵循标准。测试、lint 和其他持续集成 (CI) 工作又该如何处理？

这类任务适合使用**智能体技能**。技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将：

- 查看现有的 `quality-checks` 技能；
- 自定义技能结果的报告格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，由智能体按需加载。技能本质上是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该文件的 frontmatter 使用名称和描述定义技能，正文则概述技能用途，并说明何时调用。文件夹还可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或子文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述可以帮助 Copilot 判断何时调用技能。
3. 阅读说明，了解技能如何引导 Copilot 执行测试和 lint。
4. 注意该技能目前没有 **Results output formatting** 部分。

## 更改前先运行技能

Copilot CLI 可以直接调用技能，也可以通过自然语言调用。现在让 Copilot 运行该技能的三项检查。

1. 返回处于 Interactive 模式的筛选功能对话。
2. 发送以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

现在改进报告，使其说明执行了哪些检查、结果如何，以及工具实际报告了什么。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在运行更新后的技能，查看效果。Copilot CLI 可以重新加载已编辑的技能，无需重启对话。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前**完全相同**的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看报告，并与第一次的结果比较。

## 总结与后续步骤

你已自定义并使用了现有智能体技能。本课中，你：

- 查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了技能结果的报告格式；
- 重新加载并运行了技能。

筛选功能 PR 会包含这项更改。下一步，让 Copilot 通过 [Playwright MCP 服务器][next-lesson]直接与网站交互。

## 其他技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
