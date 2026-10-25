---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员往往需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再继续修改……需要处理的事情很多。

这正是 Copilot 智能体模式的用途。

Copilot 智能体模式可以在 IDE 中更自主地工作。它会像开发人员一样先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用智能体模式为网站添加功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但面向不同的工作方式。智能体模式在 IDE 中执行任务，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 智能体模式探索项目、查找相关文件并协调完成更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，因此你希望用户能按类别和发行商筛选。上一练习中已经添加了发行商帮助程序，现在将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

更改之前，先浏览 Tailspin Toys 网站，了解其当前功能。

该网站是一个以开发者为主题的桌游众筹平台，支持列出游戏并显示详情。它是单个 Astro 应用，在构建时将页面渲染为静态超文本标记语言（HTML）。页面通过 frontmatter 使用 Drizzle 对象关系映射器（ORM）直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问帮助程序位于 `src/lib/`；交互功能使用范围明确的 Astro `<script>` 和标准文档对象模型（DOM）API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应处于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端输出的 localhost URL 时，系统会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向托管 Web 服务器的 Codespace 的私有隧道。

### 浏览网站

网站运行后，可以探索其功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、描述、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 继续练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。各练习的模式和模型选择可能不同；需要时会在相应步骤中说明。

网站初始版本可以正常运行，但我们希望增加新功能。先查看待办事项。根据模板设置项目时，系统已自动创建一组议题；请 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好任何特定模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此教程列出该模型，希望代码建议较为一致。由于大语言模型具有概率性，你看到的建议可能与教程不同，这是正常且符合预期的情况。

> [!NOTE]
> 由于大语言模型的概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

注意 Copilot 还根据它认为对用户最有用的程度对议题进行了排序。

## 查看说明文件

让智能体生成代码之前，先查看将用于提供上下文的说明文件。这里使用[用户界面（UI）说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 浏览整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向不同文件类型说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分复杂说明，并提供示例和模板。

## 实现筛选功能

要完成筛选，至少需要对应用进行三项更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以执行这些任务，现在开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 新建对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。它应找到数据层帮助程序、UI 和测试，之后修改文件并运行测试。

   ![Copilot 探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 选项，因此无需在工作过程中立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，甚至有测试失败。这没有问题；Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且不再发现错误。

   ![Copilot 智能体模式完整聊天会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。在本练习中，你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、查找相关文件并协调完成更改。
- 在合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表增长，需要增加分页功能。使用本练习学到的方法，请 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件。
- 规划分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 在提示中提供 Copilot 实现预期功能所需的指导。
- 可能需要多次迭代 Copilot，提出修改要求并补充上下文。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：作用及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
