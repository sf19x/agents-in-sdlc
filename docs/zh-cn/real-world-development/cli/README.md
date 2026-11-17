---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** 将代理式助手带入终端。它可以探索代码库、生成代码、运行命令并连接外部工具，让你无需离开命令行即可完成工作。

在这个工作坊中，你将先安装 Copilot CLI，再实现星级评分和筛选功能。之后会探索 Plan 和 Autopilot 模式、添加自定义指令、使用 `quality-checks` 技能、连接 Playwright MCP、创建质量保证 (QA) 自定义智能体，并使用 Agent Merge 管理拉取请求。最后，你将探索用于管理上下文、模型、会话共享和可选云委派的斜杠命令。还可以参加[使用 GitHub Copilot CLI 和 Foundry 的可选系列课程][foundry]，通过三个模块准备模型、部署智能体并将其连接到网站。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI |
| [2. 添加星级评分][ex2] | 首次更改 | 显示游戏卡片的评分，审查更改并合并第一个 PR |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 从议题规划筛选功能，使用 Autopilot 构建，再审查并验证 |
| [4. 使用自定义指令引导 Copilot][ex4] | 上下文 | 探索并更新指令，再将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 更新检查报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器验证 | 在浏览器中检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 需求与覆盖 | 使用自定义智能体审查需求、测试和浏览器证据 |
| [8. 创建并合并功能 PR][ex8] | 审查与合并 | 审查功能工作并使用 Agent Merge |
| [9. 探索 Copilot CLI 实用工具][ex9] | CLI 功能 | 管理上下文、模型、会话共享，并了解可选云委派 |
| [10. 回顾与后续步骤][ex10] | 总结 | 回顾工作流和更多资源 |
| [可选：集成 Foundry][foundry] | 托管智能体 | 准备模型、构建并部署基于目录的 Backer Concierge，并将其连接到网站 |

## 先决条件

参加本工作坊前，请确保已具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 对终端/命令行操作有基本了解
- [ ] 已安装并配置 Git

> [!TIP]
> 没有付费计划？已验证学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完整完成所有路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 如果使用的是 Copilot Business 或 Copilot Enterprise，请确认管理员已启用 Copilot CLI。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
[ex9]: 9-review/
