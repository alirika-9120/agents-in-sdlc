---
title: "练习 10 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用自定义设置和其他资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流程中使用了 GitHub Copilot CLI。本研讨会中，你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选议题；
- 添加了星级评分，在转发浏览器中审查结果，并手动合并了第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中构建，并在 Interactive 模式中审查；
- 使用自定义说明引导智能体，接着自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据；
- 审查完整的筛选功能更改，并批准 Agent Merge 合并筛选 PR；
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 阶段，每个阶段都从更新后的 `main` 创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流程：**实现筛选、更新说明并应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建其 PR，你一直使用相同的对话和分支。为简化研讨会流程，这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能以新报告格式运行了单元测试、lint 和类型检查。QA 汇总这些结果，并在创建 PR 前审查需求和测试覆盖范围。

只有发现实际覆盖缺口时才需要添加测试；QA 运行后无需新增测试也可能是正确的结果。授权合并前，请审查代码和证据；更改后应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置文件，并配置了 MCP 服务器。可以在不同对话中复用这些设置，并根据团队需求调整。说明定义标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择**模式和模型**。使用 **Plan** 先制定实现方案，使用 **Interactive** 协作完成范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。常规编辑可以选择速度更快的模型；复杂工作则可选择能力更强的模型。

上下文和基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*实现，会显著影响生成结果。

## 继续探索

你已学习核心工作流程。还可以探索以下 CLI 功能：

- `/review`：请代码审查智能体分析更改；
- `/rubber-duck`：讨论问题并获取另一种看法；
- `/fleet`：并行协调相互独立的子任务；
- `/worktree`：隔离单独的任务；
- `/delegate`：将任务交给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法就是继续使用它。可以在生产代码、个人项目，或想了很久却一直没开始的小应用中使用 Copilot。与团队分享经验，也向团队成员学习。还可以继续阅读文档。

如果想继续通过终端扩展 Tailspin Toys，可以参加可选的 [Foundry Backer Concierge 系列][foundry]。若要对比其他环境，还可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
