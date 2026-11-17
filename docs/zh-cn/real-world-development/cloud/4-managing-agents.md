---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体会异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务、检查已执行的操作，并引导正在进行的会话。

在本练习中，你将：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此你将引导它在高对比度模式之外再实现浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成修改游戏的应用程序编程接口 (API)。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的步骤会因 Copilot 当前工作状态和采用的方法而异。

4. 查看右侧显示的 PR 面板，以便进一步监控 PR 和更改文件。

## 引导云智能体

查看活动任务后，要求 Copilot 在实现高对比度模式的同时添加浅色模式切换开关。

1. 选择有关添加高对比度模式的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示设置已完成并开始执行任务。出现类似示例中的消息时，表示智能体已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot 智能体页面中突出显示 Steer active session while Copilot is working 对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已回复并将请求加入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务。收到提示后，它会先完成当前步骤，再将新请求加入工作流程。

这可能需要几分钟。等待期间，可以回顾并思考目前学到的内容。

## 总结和后续步骤

本练习探索了 Copilot 智能体页面。它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪正在进行和已完成的任务、审查执行情况，并调整正在运行的任务范围或补充指导。

本练习中，你：

- 探索了用于监控云智能体任务的智能体页面。
- 引导正在进行的会话添加了其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一练习：自定义智能体][previous-lesson] | [下一练习：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
