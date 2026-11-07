---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要先做好准备。你将创建 Tailspin Toys 存储库的副本，并启动一个 [codespace][codespaces]，用于编辑说明文件和审查云智能体的工作成果。

## 设置练习存储库

你将从[模板存储库][template-repository]创建一个实例。新存储库包含练习所需的全部文件，后续练习都会使用它。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习存储库页面选择 **Use this template**，然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新存储库的路径（**organization-or-user-name/repository-name**），稍后会在练习中用到。

## 创建 codespace

接下来，使用 codespace 完成本练习系列。

[GitHub Codespaces][codespaces] 是云端开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能完善的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

codespace 可能需要几分钟才能创建完成，不过这仍比手动安装所有服务快得多。等待期间，可以先探索 GitHub Copilot 的其他功能，接下来就会用到它们。

> [!CAUTION]
> 后续练习还会继续使用此 codespace。现在先让它保持打开状态，并留在浏览器选项卡中。

> [!NOTE]
> 本研讨会可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都会预装所需组件，确保练习顺利进行。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

你已创建练习存储库的副本，并开始创建后续与 Copilot 云智能体协作时要用到的 codespace。

## 下一步

接下来，为云智能体添加自定义说明。继续学习[练习 1：自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
