---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解背景信息，就要确保它能够获取这些上下文。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解要完成什么，也知道应如何完成。

在本练习中，你将：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定的说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，这里不会运行修改前后的提示进行比较。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排演示效果。稍后审查云智能体生成的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用深色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

通过使用说明文件，可以确保 Copilot 遵循这些实践来完成任务。

### 自定义说明

自定义说明用于向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库以及注释要求。说明既可以应用于整个存储库，也可以针对特定文件或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：针对该存储库的每个请求发送给 Copilot 的单个说明文件。应包含与大多数聊天或 CLI 请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、新 UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明和带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

自定义说明文件的编写方法不止一种，项目示例体现了一种常见做法：

- 将 `copilot-instructions.md` 中的内容限制在项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，明确表达指导，并提供代码示例说明什么写法合适或不合适。

不同项目的最佳实践会有所不同，可以通过试验找到最适合自己的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的说明文件。浏览项目中的文件时，可以看到针对 [UI 更新][ui-instructions]、[Astro][astro-instructions] 等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同工作环境的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前工作环境的课程会在适用时介绍相应操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## 探索项目中的自定义说明文件

先阅读存储库中的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导；这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含 Astro、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段：它设置一个相对于存储库根目录的 glob，用于确定说明适用的文件。例如，TypeScript 测试文件（如符合 `**/*.test.ts` 的文件）都适用。
5. 阅读项目中有关创建单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这让你可以拆分较大的说明、重复使用较小文件，并为 Copilot 提供代码示例。这里的路径相对于说明文件所在目录，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需编辑一次：添加一条项目级规则，要求使用 TSDoc 文档注释和文件级注释头。之后在本工作环境中分配议题时，云智能体会读取这项规则。

编辑前先创建一个分支。云智能体会读取议题目标分支中的说明，但仍应通过提交妥善保存更改。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于提供项目级信息。现在添加编码标准，改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意该部分尚未说明代码内文档要求，这也是生成的帮助程序没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别的 `.github/copilot-instructions.md` 中编写全局指导，也可以使用 `*.instructions.md` 针对语言、文件类型或任务添加说明。项目级标准（例如文档注释规则）适合放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中将把议题分配给 Copilot，Copilot 会从 `main` 创建分支，因此必须先将说明更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后，云智能体处理下一练习中分配的议题时就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上完成本研讨会，可以跳过分支步骤并直接提交到 `main`。这里创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已完成以下工作：

- 探索了项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定的说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。

之后审查云智能体生成的拉取请求时，可以看到这项标准的实际效果。

接下来，将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
