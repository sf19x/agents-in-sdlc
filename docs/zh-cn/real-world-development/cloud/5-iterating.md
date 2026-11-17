---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一练习：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你让 GitHub Copilot 执行了几项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在查看代码更改，必要时向 Copilot 提供反馈。

### 场景

加入生成式人工智能 (AI) 并不会改变软件设计和开发运维 (DevOps) 的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。现在审查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改，然后再允许团队其他成员审查。

## GitHub Copilot 云智能体的安全性

Copilot 云智能体会异步且自主地执行任务，因此受到以下安全限制：

- Copilot 只能读取存储库，并且**只能**写入用于执行任务的分支。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 任何 GitHub Actions 工作流都必须经人工批准后才能运行。
- [默认情况下会限制外部资源访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议 (MCP) 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，也就是为代码添加文档的 PR。使用 GitHub.com 中的标准 PR 界面审查它。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以选择[自定义或禁用云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成；这可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改可能会不同。

   查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件头注释。这些要求来自练习 2 中查看的自定义指令文件。

6. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 检查区域的队列中。正常情况下，单个 Astro 应用的项目检查会通过；这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像 @ 提及团队成员一样，在 PR 评论或代码内联评论中提及 Copilot。Copilot 会读取评论并启动新会话处理请求。由于结果并非确定，不适合规定唯一的提示文本；可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，查看 Copilot 如何启动新会话并执行更改。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request 按钮](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，确认 PR 中出现了新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。确认完成后，将 PR 从草稿状态改为准备审查，再合并到 `main`。

![将 PR 从草稿状态改为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在 PR 检查区域的队列中。正常情况下，单个 Astro 应用的项目检查会通过；这可能需要几分钟。
9. **可选：**可以在 Codespace 中检出该分支，手动测试相关游戏功能。打开终端，并将 `<branch-name>` 替换为 Copilot 创建的分支名称（例如 **copilot/fix-8**），然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。你可以继续通过 PR 评论要求云智能体进行调整，例如修改显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时尚未更新自定义指令）。与前面的操作一样，在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后审查使用无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在智能体控制中心请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现时，特别留意：
   - 用于切换模式的界面组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 检查区域的队列中。正常情况下，单个 Astro 应用的项目检查会通过；这可能需要几分钟。
10. **可选：**可以在 Codespace 中检出该分支，手动测试无障碍功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名称，然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   随后启动应用，在浏览器中测试高对比度和浅色模式切换开关，并确认偏好设置在页面重新加载后仍然保留。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进机会，可以像处理前面的 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习：继续委派任务

云智能体适合处理实际待办事项，让你可以继续其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的练习。在这些练习中，你：

- **检查了存储库中已有的自定义指令文件**，以便稍后查看这些指令对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并查看它如何设置环境、规划并异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作环境的练习。还可以通过其他工作环境继续探索：

- 🖥️ **[VS Code 工作环境](../../vscode/)**：直接在集成开发环境 (IDE) 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)**：通过终端交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体委派。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一练习：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
