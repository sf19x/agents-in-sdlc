---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，先准备好环境。你将创建 Tailspin Toys 存储库副本、启动用于练习的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置实验存储库

通过[模板存储库][template-repository]创建自己的副本。副本包含实验所需文件，后续练习会在其中进行。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template**，再选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建存储库。

   ![存储库模板设置输入页面](../../../_images/ex0-repository-settings.png)

4. 记下新建存储库的路径（**组织或用户名/存储库名称**），后续练习会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建存储库时，GitHub 会自动为你创建一组待处理议题。整个工作坊都会使用这些议题，无需自行创建。

## 创建 Codespace

接下来使用 Codespace 完成实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供支持多种编程语言、扩展和工具的完整集成开发环境 (IDE)。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

   ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建可能需要几分钟，但比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习还会使用此 Codespace。暂时不要关闭浏览器标签页。

> [!NOTE]
> 本工作坊可在 Codespace 或本地[开发容器][dev-containers]中运行。两者都预装了所需依赖项。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

若要使用 GitHub Copilot Chat 代理模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时通常已安装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能还需要使用 GitHub 帐户进行身份验证。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 <kbd>Enter</kbd>。这会启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 帐户。按照提示完成身份验证。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证后，Copilot Chat 窗口应会显示出来。

## 总结

你已创建实验存储库副本，并开始创建后续编写代码时要使用的 Codespace。

## 下一步

接下来开始使用 Copilot。在[练习 1：自定义指令][next-lesson]中，为 Copilot 说明项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
