---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。若任务需要以特定方式完成，或 Copilot 需要背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供上下文，其中一种重要方式是使用[说明文件][instruction-files]指导代码生成。

在本练习中，你将：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定的说明文件传递给 Copilot。
- 使用当前说明生成筛选功能的第一部分，即发行商帮助程序。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能基础更改提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设定的标准。Copilot 的行为具有概率性而非确定性。本练习旨在观察更新说明后的行为趋势，而不是要求输出与示例逐字一致。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

通过使用说明文件，可以确保 Copilot 遵循这些实践来完成任务。

### 自定义说明

自定义说明向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库和注释要求。说明既可以应用于整个存储库，也可以针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：针对该存储库的每个请求发送给 Copilot 的单个说明文件。应包含与大多数请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定语言（例如 TypeScript 或 Astro）、文件类型或任务（例如创建 UI 组件或单元测试）提供说明。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明和带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

项目示例展示了一种常见做法：

- 将 `copilot-instructions.md` 中的内容限制为项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用自然语言明确表达要求，并提供代码示例说明合适和不合适的写法。

说明文件没有唯一正确的组织方式。通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的说明文件。浏览项目时，可以看到针对 [UI 更新][ui-instructions]、[Astro][astro-instructions]等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同工作环境的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前环境的课程会在适用时介绍相应操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

先阅读存储库提供的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导；这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含 Astro、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 阅读项目中有关创建单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分成可复用的小文件，并为 Copilot 提供示例。这里的路径相对于说明文件所在目录，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 创建更改分支

先为更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 继续操作前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。各练习的模式和模型选择可能不同；需要时会在相应步骤中说明。

先使用当前说明文件发送提示，观察现有说明下生成的结果。之后更新说明，再发送相同提示进行比较。

1. 关闭之前练习遗留的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道帮助程序应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，使 Copilot 可以修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改帮助程序文件和测试。
6. 注意建议的帮助程序是一个类型化函数，以 `db` 客户端为第一个参数，并返回类型化的发行商数组。这符合 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。这种情况也没关系；本练习重点是观察更新说明后行为是否更一致。

## 添加新的存储库标准

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
> 存储库级的 `.github/copilot-instructions.md` 适合放置项目级标准；`*.instructions.md` 则可针对语言、文件类型或任务提供具体指导。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦在正确文件上。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一练习会继续使用的实际代码。

## 提交说明并推送分支

说明文件和其他项目资源一样，应通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和帮助程序更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新及 Copilot 为帮助程序基础所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并关联待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建用于关闭该议题的 PR，并完成合并。

> [!NOTE]
> 这是首次让 Copilot 代表你在 GitHub 上执行操作。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub；按照提示授权即可。后续练习会介绍此连接使用的 Model Context Protocol。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选择 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中既有说明更新，也有新的发行商帮助程序基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，审查命令，并在适当时选择 **Allow**。
6. PR 创建后，使用以下提示请 Copilot 合并 PR，并在本地切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，审查命令，并在适当时选择 **Allow**。

现在，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结和后续步骤

你已探索 Copilot 如何获取项目说明文件中的上下文，并在 VS Code 中使用 Copilot Chat：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定的说明文件传递给 Copilot。
- 发送代码生成提示，观察 Copilot 根据**现有**说明生成的结果。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察建议代码如何采用新标准。
- 将说明更新和帮助程序基础提交到 `main`，供下一练习继续使用。

接下来，Copilot 将通过[智能体模式][next-lesson]在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
