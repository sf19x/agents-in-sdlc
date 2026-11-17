---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者往往需要查找相关文件、进行修改、运行测试、调试，再找出遗漏的文件并继续修改。

Copilot 代理模式可以帮助处理这类工作。它能够在集成开发环境 (IDE) 中更自主地运行：先探索项目结构，再执行必要更改、运行测试，并自动修复发现的问题。现在使用代理模式为网站添加功能。

> [!NOTE]
> 名称相似，但代理模式和云智能体提供不同体验。代理模式在 IDE 中工作，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot 代理模式探索项目、查找相关文件并协调更改。
- 使用 Copilot 代理模式跨界面和数据层实现新功能。
- 在合并到代码库前审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户能够按类别和发行方筛选。上一练习已添加发行方辅助函数，现在使用 Copilot 代理模式完成剩余的数据层、界面和测试工作。

## 运行 Tailspin Toys 网站

更改之前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，在构建时将页面渲染为静态超文本标记语言 (HTML)。页面通过 Drizzle 对象关系映射 (ORM) 在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准文档对象模型 (DOM) API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站。

1. 返回 Codespace。上一练习结束后，你应位于 `main` 分支。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
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
> 如果出现提示打开 `http://localhost:4321` 的对话框，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），再选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 网站的私有隧道。

### 浏览网站

网站运行后，查看其主要功能：

- **主页**：列出棋盘游戏的标题、图片和描述。
- **游戏详情页**：选择游戏后可查看标题、描述、发行方和类别等详细信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用模式和模型可能不同，相关步骤会具体说明。

网站已有初始实现，现在要添加新功能。先查看从项目模板自动创建的议题待办事项，并让 GitHub Copilot 帮助梳理优先级。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)
2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此在步骤中指定该模型，希望获得相对一致的体验。由于大型语言模型 (LLM) 具有概率性，实际建议可能与示例不同，这是正常现象。

> [!NOTE]
> Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问存储库中的议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

Copilot 会根据它认为对用户最有用的程度为议题排序。

## 查看指令文件

生成代码前，先查看将用于提供上下文的指令文件。项目的[用户界面 (UI) 指令][ui-instructions]说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 针对 Astro、Tailwind CSS 和 Drizzle 等文件类型的指令链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、拆分大型指令集，并提供代码示例。

## 实现筛选功能

完成筛选至少需要对应用进行三类更改：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面并添加筛选界面。

合并前还需要运行并通过测试。Copilot 代理模式可以执行这些任务。

1. 继续当前对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)
4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，根据待办事项中的议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件。它会找到数据层辅助函数、界面和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 中要求确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 可能会暂停，也可能在迭代过程中出现测试失败。它会继续调整代码并运行测试，直到不再检测到错误。

   ![Copilot 代理模式完成后的 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)
9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终审查 Copilot 或其他人工智能 (AI) 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了新功能。你了解了如何：

- 使用 Copilot 代理模式跨界面和数据层实现新功能。
- 让 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 在合并到代码库前审查 Copilot 代理模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索：实现分页

游戏列表不断增长后，就需要分页。运用本练习学到的知识，让 Copilot 为网站添加分页。可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定由用户选择每页大小，还是使用固定大小。
- 在提示中提供 Copilot 所需的指导，确保实现符合你的要求。
- 可能需要通过补充上下文和要求更改来迭代。这是使用 Copilot 的常见方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
