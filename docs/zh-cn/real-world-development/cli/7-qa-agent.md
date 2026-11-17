---
title: "第 7 课 - 创建并使用质量保证 (QA) 智能体"
description: "创建质量保证 (QA) 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，建立清晰且可复用的质量保证 (QA) 流程。

在本课中，你将：

- 探索自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其结果。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用角色。配置文件说明智能体的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义功能各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告结果。它不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

在打开功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。配置文件将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 记录了预期的 QA 流程和边界，避免它在你只想验证功能时进行不完整或过度宽泛的更改。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但添加后可为智能体提供清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果配置文件遗漏任何职责或边界，请先让默认智能体修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选功能对话并选择 QA，以便它使用已有的议题和规划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 配置文件刚创建后，智能体可能不会显示在可用列表中。上述命令仍会启用它。

2. 使用以下提示让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体完成检查。
4. 阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加可复用的专用角色，并审查了它的工作。具体来说，你：

- 探索了自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选议题审查其结果。

实现、技能更新、QA 配置文件、测试和验证报告现已准备好审查。接下来，将它们整合到功能 PR 中，并使用 [Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
