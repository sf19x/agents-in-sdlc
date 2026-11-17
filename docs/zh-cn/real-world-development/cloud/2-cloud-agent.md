---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一练习：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿拉取请求 (PR)，供开发者审查。这样可以将繁琐任务交给智能体处理，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有留下完善的文档，事实上几乎完全没有文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。当前不要求完善的实现，只需要一个可用于用户体验验收测试的版本。虽然该任务目前阻碍了相关工作，但还有优先级更高的议题。

这两类任务可能很容易被延后，因此适合分配给 Copilot 云智能体。它可以异步处理任务，开发者则继续做其他工作，之后再回来审查结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台处理任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度，设置环境并开始工作。你可以在会话进行时或完成后查看过程。解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确指令范围的重要性

GitHub Copilot 不会替代思考。看似简单的操作也可能包含复杂情况，因此应[谨慎限定交给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为人工智能 (AI) 配对程序员：分阶段工作、学习、试验并不断调整。加入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的效果；否则，可以先快速查看。分配任务前，以只读方式检查存储库中的指令文件，稍后便能了解它们如何影响结果。

1. 在存储库的 GitHub Web 界面或正在运行的 Codespace 中打开以下文件：
   - `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
   - `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

将 `Code lacks documentation` 议题分配给云智能体后，观察生成的 PR 是否包含 TSDoc 文档注释、TypeScript 约定和文件头注释。这些要求来自上述指令文件，后续练习会再次检查。

## 设置 Copilot 云智能体的开发环境

生成代码通常需要特定环境和初始化脚本。将任务分配给类似软件工程师 (SWE) 的 Copilot 时，也需要准备开发环境。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以通过创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中配置云智能体开始工作前要执行的步骤。该工作流可安装所需开发工具和依赖项。本实验已预先配置它，其中包括 Node.js、项目依赖项、端到端测试所需的浏览器，以及用于单个 Astro 应用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可让你在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为执行*这些步骤*所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流相似，有几个重要特点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流，以便提前验证，而不必等待 Copilot 启动它。

## 添加文档

许多项目缺少文档或文档已过时，这类技术债务会降低工作效率，也让维护代码和引入新开发者更困难。Copilot 可以帮助生成文档，因此适合将这项任务分配给云智能体。后续练习会审查它生成的内容。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的 PR。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新页面。
12. 等待 Copilot 创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表，因为它需要创建环境、运行前面介绍的工作流、分析项目并确定处理方式。

13. 查看 Copilot 将执行的任务列表。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的指示器。

16. 浏览实时会话，查看 Copilot 如何探索代码、了解当前状态、确定计划并生成代码。

这可能需要几分钟。云智能体的主要目标之一是异步执行任务，让开发者可以继续处理其他工作。现在利用这一能力，再分配一项任务，然后转而为应用添加功能。

## 为游戏详情页添加相关游戏部分

云智能体可以分担工作，让你专注于其他任务。即使为设计团队添加相关游戏部分不一定耗时，也可以将任务交给云智能体处理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二项请求。和软件工程师类似，云智能体会独立工作；你可以等任务完成后再审查结果。现在转而创建并使用自定义智能体。

## 总结和后续步骤

本练习探索了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给 Copilot，让它异步处理技术债务、开发功能或协助迁移代码框架等任务。

本练习中，你了解了：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作。接下来创建并使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并可读取前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一练习：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
