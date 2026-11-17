---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能 (AI) 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识固化为可复用智能体，例如遵循[《Web 内容无障碍指南》(WCAG)][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段；后面是用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体和智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何完成任务。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。和 AI 相关的其他工作一样，需要测试并迭代，找到适合环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前深色主题的文本与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求提供可以开关的高对比度模式。

无障碍功能十分重要，因此需要尽快实现。你将使用自定义智能体生成此功能。

在本练习中，你将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用该智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目中已创建无障碍自定义智能体。先查看文件，了解它将如何引导 Copilot。

1. 返回 Codespace，在终端中从 `main` 创建新分支，以便将无障碍工作与练习 3 的筛选 PR 分开：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. 打开 `.github/agents/accessibility.md`。
3. 查看带有 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

4. 浏览后续内容，查看其中说明的：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该会话会遵循相应指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；相关步骤会具体说明。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**（如果尚未选中）。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表。它会显示当前活动的智能体，默认是 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会以内联差异的形式显示文件路径和更改内容，供你审查。
6. 随着编辑进行，Chat 顶部的 **Files changed** 指示器会更新，显示本会话中智能体修改的文件。

> [!NOTE]
> 这个过程可能需要几分钟。Copilot 会实际修改存储库，例如 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项正在进行的工作。

## 总结和后续步骤

本练习探索了 GitHub Copilot [自定义智能体][custom-agents]，也就是针对特定任务和领域定制的专用 AI 助手。自定义智能体可以将团队专业知识和标准固化为可复用的配置，帮助 Copilot 更有效地完成特定工作。

本练习中，你了解了：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 代理模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一练习：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一练习：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
