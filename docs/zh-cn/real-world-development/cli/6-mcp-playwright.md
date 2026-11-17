---
title: "第 6 课 - 使用 Playwright 模型上下文协议 (MCP) 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面已经提到，开发工作不只是编写代码。还需要处理数据和外部服务，并为 Copilot 接入更多自动化工具；模型上下文协议 (MCP) 服务器正是为此设计的。MCP 服务器可以扩展 Copilot CLI 的内置能力，为它提供更多工具和服务。

在本课中，你将：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 让智能体驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证界面更新还需要实际交互。你将允许 Copilot 像用户一样使用正在开发的网站，进一步自动验证更改，从而提高对功能行为的信心。

## 什么是模型上下文协议 (MCP)？

[MCP][mcp-blog-post] 为人工智能 (AI) 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，从而获取最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，每个服务器都提供一组可供智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的应用程序编程接口 (API)。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并贡献生态系统中的项目。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚未配置，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 确认服务器类型为 **STDIO**，按 <kbd>Enter</kbd>，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已手动确认功能行为。现在使用 Copilot CLI 中的 Playwright MCP 服务器执行同样的验证。

1. 使用以下提示，要求 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot 通常能自行判断是否使用 MCP 服务器，因此不一定需要特别指定。不过，明确指出工具有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行的步骤，确认功能行为。
3. 阅读报告，确认所有功能都按预期运行。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结和后续步骤

你已使用 Playwright MCP 服务器和 Copilot CLI 在真实浏览器中体验功能。具体来说，你：

- 了解了模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果此前未配置，则添加了 Playwright MCP 服务器。
- 让智能体驱动浏览器并体验筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门的角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
