---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一练习：先决条件][previous-lesson] · [下一练习：Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可以提供相关指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

在本练习中，你将：

- 探索如何通过存储库自定义指令和路径范围指令文件，为 Copilot 提供项目上下文、编码指南和文档标准。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会运行“更新前后”提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。后续审查云智能体创建的拉取请求时，可以看到指令文件如何影响结果。

## 指令文件

### 场景

Tailspin Toys 团队有一组开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 界面应采用深色模式并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应有一段注释，说明文件用途。

通过指令文件，可以确保 Copilot 按照这些实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导的文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件有许多做法。本项目中的示例展示了常见方法：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供代码示例。

没有唯一正确的指令编写方法。可以通过实践找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的指令文件集合。查看本项目中的文件时，会发现其中包括[界面更新指令][ui-instructions]和 [Astro 指令][astro-instructions]等。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；相关工作环境会说明具体方法。
>
> 需要模板或起点时，可以探索包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## 探索项目中的自定义指令文件

查看此存储库包含的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。可以在 GitHub Web 界面或正在运行的 Codespace 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 中还包含 **GitHub Actions Workflows** 子部分。这些指导适用于 Copilot 在此存储库中的交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。这里有 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看 `applyTo` frontmatter，它将指令限定在匹配 `**/*.test.ts` 的文件。
5. 查看此文件中针对本项目单元测试的具体指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目文件的链接。这样可以将大型指令集拆分成可复用的小文件，并让 Copilot 参考项目示例。这里的路径相对于该指令文件，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库标准

接下来要编辑一次：添加项目级规则，要求代码中使用 TSDoc 文档注释和文件头注释。后续练习中，云智能体处理分配给它的议题时会读取这项规则。

编辑前先创建分支。云智能体会从议题目标分支读取指令，但仍建议单独提交本次更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于提供项目级信息。现在添加一项编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约位于第 27 行。查看现有编码标准；它尚未要求添加代码文档，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令可以放在存储库级 `.github/copilot-instructions.md` 中，供整个项目使用；也可以放在适用于特定语言、文件类型或任务的 `*.instructions.md` 文件中。项目级文件适合记录诸如文档注释规则等全局标准。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支上的指令。下一练习中，将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。后续练习中，云智能体就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟实际项目的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何读取项目指令文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，可以在云智能体生成的拉取请求中看到该标准的效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
