---
title: "练习 3 - 使用 Playwright 模型上下文协议 (MCP) 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一练习：代理模式][previous-lesson] · [下一练习：自定义智能体 →][next-lesson]

你刚使用代理模式构建了筛选功能。在创建拉取请求前，应先确认功能能在浏览器中正常运行。此处将连接 **Playwright 模型上下文协议 (MCP) 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建拉取请求。

在本练习中，你将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

代理模式可以访问编辑器之外的资源，从而变得更强大。模型上下文协议 (MCP) 是 Copilot 与外部工具和服务通信的标准方式。

![代理模式与上下文、大型语言模型 (LLM) 和工具的交互示意图，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为人工智能 (AI) 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以通过资源获取最新信息，并通过工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有应用程序编程接口 (API) 或 npm 包）的通信。每个 MCP 服务器都提供一组可供 AI 智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让智能体可以创建存储库、更新存储库、管理议题和拉取请求等。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让智能体可以浏览网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器可提供不同工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并贡献生态系统中的项目。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查源代码、验证发布者并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 查看已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板已包含 `github` 条目，因此 Copilot 能在上一练习中读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件如下所示：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此所需浏览器通常已安装。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问你是否信任该服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要其工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开页面、导航、应用筛选条件并读取结果，然后总结观察内容，无需你离开对话手动操作网站。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而非截图。智能体会根据带标签的结构化元素（按钮、链接和列表项）进行推理，与辅助技术读取页面的方式类似，因此功能检查也能顺便进行简单的无障碍检查。

服务器已连接后，先确认应用正在运行，再让 Copilot 测试刚完成的筛选功能。

1. 确认开发服务器正在运行；如果已停止，请重新启动。然后使用以下提示让 Copilot 测试筛选功能：

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

2. 根据议题中的验收标准审查 Copilot 的报告。如果报告显示缺少浏览器，请让 Copilot 运行 `npx playwright install chromium`，然后重试测试；如果发现筛选异常，继续询问或要求 Copilot 在创建拉取请求前修复代码。

> [!NOTE]
> 测试要求应用运行在 `http://localhost:4321`。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以发布 `filtering-vscode` 分支并创建拉取请求 (PR)，供团队审查。

1. 打开 Codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示 Copilot 更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。也可以让 Copilot 使用已连接的 GitHub 工具查找相关议题并创建关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 在智能体下拉列表中确认选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证，请按照提示操作。
5. 确认 Copilot 搜索议题、找到相关议题并创建了 PR。
6. 选择 Copilot 提供的链接审查 PR，但暂时**不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 在交付筛选功能前，使用它在浏览器中测试了功能。
- 发布了分支并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 也可以用其他方式扩展 Copilot。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
