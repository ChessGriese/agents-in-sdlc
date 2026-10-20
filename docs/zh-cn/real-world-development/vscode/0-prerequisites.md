---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要完成准备工作。你将创建 Tailspin Toys 存储库的副本，启动一个用于练习的 [Codespace][codespaces]，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验存储库

要创建用于编写代码的存储库副本，请从[模板][template-repository]创建一个实例。新实例将包含实验所需的全部文件，你将在整个练习过程中使用它。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Tailspin Toys 存储库模板页面上的 Use this template 按钮。](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可在有权使用 GitHub Copilot 的组织中创建新存储库。

    ![Create a new repository 页面中用于设置新存储库的模板信息。](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**组织或用户名/存储库名称**），后续实验中会用到它。

> [!NOTE]
> **待办工作已准备就绪**
>
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 议题。整个工作坊都会使用这些议题，无需自行创建。

## 创建 Codespace

接下来，使用 Codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![GitHub 存储库页面上用于打开克隆和开发选项的 Code 按钮。](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![GitHub Codespaces 中创建新 Codespace 的界面。](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，不过比手动安装所有服务快得多！等待期间，可以探索 GitHub Copilot 的其他功能，稍后我们会继续介绍。

> [!CAUTION]
> 后续练习还会用到该 Codespace。现在请先让它保持打开状态，并留在浏览器的一个选项卡中。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行，两者都已安装顺利完成练习所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

要使用 GitHub Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。如果使用 GitHub Codespace，该扩展应该已安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证才能启用它。

1. 返回 Codespace。
2. 如果编辑器右侧未显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 Enter。这会激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 账户。按照说明完成身份验证。

    ![Copilot Chat 的登录提示，引导用户通过 GitHub 账户完成身份验证。](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜！你已创建实验存储库的副本，并开始创建 Codespace。开始编写代码时会用到它。

## 下一步

现在开始使用 Copilot。[继续学习练习 1 - 自定义说明][next-lesson]，了解如何向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
