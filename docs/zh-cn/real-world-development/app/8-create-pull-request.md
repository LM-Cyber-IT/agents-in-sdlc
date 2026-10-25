---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试已保存在同一分支上。现在需要一并审查这些内容并创建拉取请求（PR）。你已自行合并星级评分 PR；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分为几个独立的 PR。为简化工作坊流程，这里将整个筛选和质量工作流保留在同一会话和分支中，并纳入此 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动执行合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 审查后再授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证功能。现在，Tailspin Toys 希望自动执行剩余的 PR 工作，同时仍由开发人员控制合并授权。

## Agent Merge 简介

**Agent Merge** 可在 GitHub Copilot app 中自动处理拉取请求合并前的剩余工作。启用后，会话会读取拉取请求并处理阻塞项，例如修复失败的持续集成（CI）检查、响应审查意见，以及在需要时变基；GitHub 允许后，它会合并拉取请求。它在后台运行，应用重启后仍会继续，并在拉取请求合并后自动关闭。

此前，你一直自行选择 **Merge pull request**。Agent Merge 可以承担这项工作，但它编辑代码和合并的能力仍需要你明确授权。授予合并权限前，先审查它获准执行的操作及相关工作。

## 使用 Agent Merge 管理 PR

所有代码都已完成并审查后，启动 Agent Merge 来管理 PR 流程。先检查差异和验证证据，再明确授予合并权限。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅在下拉菜单中选择该选项并不会启动流程。

Agent Merge 随即启动，并将：

- 创建包含标题和说明的拉取请求。
- 如果会话从议题启动，则在说明正文中引用相关议题。
- 对目标分支执行变基或处理潜在合并冲突。
- 监视 CI 流程，确保所有检查通过。
- 监视其他开发人员或 Copilot 代码审查提供的反馈，并进行更改以解决相关意见。
- 在所有条件满足后自动合并 PR（如果你授权了此操作）。

在授权合并前，先审查 PR 的差异和验证证据。

5. 检查 PR 的更改差异及验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体执行合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进进度并解决报告的阻塞项。继续之前，确认 PR 状态为 **Merged**；已启用合并权限或智能体仍在运行，都不能证明合并已完成。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续前请先解决这些阻塞项。

## 总结和后续步骤

你已自动执行开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。在本课中，你：

- 了解了 Agent Merge 及其如何自动执行合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，[检查现有画布并创建分类画布][next-lesson]，探索与智能体协作查看、规划和可视化工作的方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
