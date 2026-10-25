---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿拉取请求，供开发人员审查。这样可以把繁琐的任务交给智能体处理，加快开发流程，让开发人员专注于更重要的工作。

在本练习中，你将使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 希望处理一些技术债务。最初受聘开发网站的承包商没有留下合适的文档，实际上完全缺少文档。团队首先希望为应用程序中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望在每款游戏的详情页上展示相关游戏，即同一类别的其他游戏，让访客可以继续浏览。团队目前不要求完善的实现，只希望有可供验收测试的用户体验原型。该工作暂时被阻塞，团队还有优先级更高的议题。

这类任务很容易被推迟，因此适合分配给 Copilot 云智能体。智能体可以异步处理工作，让开发人员继续完成其他任务，之后再回来审查结果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体][copilot-agents]可以像人类开发人员一样在后台执行任务。你可以通过多种方式委派工作，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话执行过程中或完成后查看详情。方案准备好供你审查时，Copilot 会在拉取请求中通知你。

## 明确界定任务范围的重要性

GitHub Copilot 并没有魔法，使用它并不能省去思考。即使看似简单的操作，深入了解后也可能很复杂，因此应仔细界定云智能体的任务范围，并参考[云智能体最佳实践][cloud-agent-best-practices]。将它视为人工智能（AI）结对程序员：分阶段工作、学习、试验并持续调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已经完成过自定义说明练习，就见过 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看。将工作分配给云智能体前，只读浏览存储库中已有的说明文件，以便稍后观察它们的影响。

在存储库的 GitHub 网页界面或运行中的 Codespace 中打开说明文件，以便之后观察它们的影响：

1. 打开 `.github/copilot-instructions.md`，查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释方面的要求。
2. 打开 `.github/instructions/unit-tests.instructions.md`，注意 `applyTo` frontmatter 将这些说明限定为 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求（PR）是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述说明文件；后续练习审查 PR 时还会再次提到。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行设置脚本，以确保环境状态正常。分配给 Copilot 的任务也一样，因为它会像软件工程师一样处理任务。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建专用设置工作流并配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便访问所需开发工具和依赖项。实验开始前已预先配置该工作流，以便顺利完成本研讨会。它会为单个 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 权限应仅满足*你的步骤*所需的最低要求。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动执行此操作。
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来像其他 GitHub 工作流文件，但有几个要点：

- 其中包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以在等待 Copilot 运行前验证工作流是否成功。

## 添加文档

大多数项目的文档要么过时，要么完全缺失。这类技术债务常被搁置，会降低工作效率，让代码库维护和新开发人员入职变得更困难。Copilot 擅长创建文档，因此很适合将这项工作作为议题分配给云智能体。它会在后台生成所需文档，后续练习中再回来审查。

1. 在新的浏览器选项卡中访问 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求（PR），其标题类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 审查待办事项及 Copilot 将完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口，才能看到更新后的状态指示器。

16. 浏览实时会话，查看 Copilot 如何解决问题，包括探索代码、了解当前状态、思考并选择适当计划，以及编写代码。

这可能需要几分钟。云智能体的主要用途之一是异步执行任务，让我们可以继续处理其他工作。现在利用这一点，将另一个任务分配给云智能体，然后把注意力转向为应用添加功能。

## 在游戏详情页添加相关游戏区域

GitHub Copilot 云智能体的一大优势是可以并行分工：你可以处理一组任务，让它处理另一组任务。即使为设计团队添加相关游戏区域不需要很长时间，也可以把这段时间用在其他工作上。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

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

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体会像软件工程师一样工作，因此无需持续监控；等工作完成后再审查即可。现在转向创建和使用自定义智能体。

## 总结和后续步骤

本课探索了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理工作。Copilot 可以帮助解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你探索了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以转向创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并支持前面练习中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
