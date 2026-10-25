---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支上。现在一起审查这些更改并创建拉取请求。你已亲自合并星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 正常情况下，功能、说明更新、技能更新和 QA 智能体应分别放入几个 PR。为了简化研讨会流程，筛选功能和质量工作流都保留在同一段对话和分支中，并合并到同一个 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动管理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你已使用 Copilot 规划、实现和验证功能。现在 Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发人员掌控是否授权合并。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题——修复失败的持续集成（CI）检查、回复审查意见，并在需要时变基——然后在存储库允许的情况下启用 GitHub 自动合并。

此前你一直自行选择 **Merge pull request**。Agent Merge 可以接管这项工作。启用前请审查更改，并确认已准备好合并。

## 使用 Agent Merge 管理 PR

所有代码都已完成，现在一起审查、创建 PR，并让 Agent Merge 处理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动处理开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。在本课中，你：

- 了解了 Agent Merge 及其如何自动管理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，探索更多实用的 Copilot CLI 斜杠命令，以管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
