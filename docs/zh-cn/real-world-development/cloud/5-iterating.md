---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你已与 GitHub Copilot 协作完成多个任务，以改善用户体验并添加功能。你请 Copilot 为代码添加文档、为设计团队实现相关游戏功能，并添加无障碍功能，包括高对比度和浅色模式切换。现在审查代码更改；如有需要，向 Copilot 提供反馈以改进结果。

### 场景

如前所述，生成式人工智能（AI）不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。现在审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能生成的更改，然后再启用团队其他成员的审查。

## 安全性与 GitHub Copilot 云智能体

Copilot 云智能体会异步、无人监督地执行任务，因此有一些安全限制：

- Copilot 对存储库只有读取权限，仅能写入用于编写代码的分支。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立的临时环境。
- 运行 GitHub Actions 工作流前必须由人工批准。
- 默认情况下，[对外部资源的访问受到限制][agent-firewall]，包括 MCP 服务器。

## 审查生成的文档

先检查 GitHub Copilot 云智能体创建的第一个拉取请求（PR），即为代码添加文档的 PR。使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源（包括外部 MCP 服务器）的访问受限。你可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。此时请等待 Copilot 完成。可能需要几分钟，可以稍作休息或回顾之前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中应包含新添加的 TSDoc 文档注释和其他文档。具体更改因结果而异。审查时，检查 TSDoc 文档注释、TypeScript 约定和文件注释头；这些要求来自练习 2 中查看的自定义说明文件。
6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应在 PR 检查区域中排队。如果一切正常，单个 Astro 应用的项目检查会通过。完成可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向操作。你可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话进行处理。由于结果具有不确定性，无法规定确切提示内容；可以要求 Copilot：

- 在每个代码文件开头添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用的结构。

1. 添加评论请求 Copilot 修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上述建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，查看 Copilot 执行工作。注意 Copilot 会启动新会话进行修改。
3. 可以选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认结果后，可以将 PR 从草稿转换为可审查状态，并合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来返回 Copilot 为“在游戏详情页显示相关游戏”议题生成的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的指示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应在 PR 检查区域中排队。如果一切正常，单个 Astro 应用的项目检查会通过。完成可能需要几分钟。
9. **可选：**在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。与之前一样，可以通过评论请求云智能体迭代修改，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此议题时还没有更新自定义说明）。在 **Conversation** 选项卡中添加评论后，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应在 PR 检查区域中排队。如果一切正常，单个 Astro 应用的项目检查会通过。完成可能需要几分钟。
10. **可选：**在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后仍保留设置。

注意无障碍自定义智能体如何引导 Copilot 按照无障碍最佳实践实现功能。如果发现问题或改进机会，可以像处理前面的 PR 一样，在评论中标记 **@copilot** 请求修改。

## 可选练习——继续委派任务

云智能体适合处理真实待办事项，让你可以把注意力转向其他工作。可以为存储库创建更多议题并分配给 Copilot，培养这种工作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问帮助程序添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的研讨会。通过这些课程，你：

- **检查了存储库自带的自定义说明文件**，了解这些说明如何影响云智能体的输出。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、规划并异步执行工作。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代了云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 审查和后续步骤

云智能体工作环境的研讨会已完成。其他工作环境可以补充你在这里练习的内容：

- **[VS Code 工作环境](../../vscode/)**——在 IDE 中使用 Copilot Chat 智能体模式并集成 MCP。
- **[CLI 工作环境](../../cli/)**——在终端中交付两项经过审查的更改，并使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
