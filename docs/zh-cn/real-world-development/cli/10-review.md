---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了已准备好的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义说明指导智能体，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol（MCP）服务器，并通过真实浏览器探索筛选功能。
- 创建并选择了质量保证（QA）自定义智能体，以评估需求、测试覆盖率、技能结果和浏览器验证证据。
- 审查了完整的筛选更改，并授权 Agent Merge 合并筛选功能 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 完成的工作

本研讨会包含两个 PR 里程碑，各自从更新后的 `main` 分支创建：

1. **星级评分**：在游戏卡片上显示已有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并加入相关测试。

从规划筛选功能到创建 PR，你一直使用同一段对话和分支。为了简化研讨会流程，这些工作合并到了一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按照新的格式报告结果。QA 汇总这些结果，并在 PR 创建前审查需求和测试覆盖率。

新增测试应弥补真实缺口；如果 QA 运行后发现无需添加测试，也可能是合理结果。授权合并前，请审查代码和证据；代码更改后，请更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复任务，自定义智能体用于定义专门角色，MCP 服务器用于连接外部工具。应检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先思考方案，使用 **Interactive** 处理需要持续参与的集中式更改，使用 **Autopilot** 处理范围明确的任务。常规修改可以选择速度更快的模型，复杂工作则选择能力更强的模型。

上下文仍然和基础设施一样重要。清楚说明要构建*什么*、*为什么*构建，以及*如何*构建，会显著影响输出结果。

## 继续探索

核心工作流已完成。还可以了解以下 CLI 功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一个视角。
- `/fleet`：协调多个独立子任务并行运行。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最好方式，就是持续使用它。将它用于生产代码、个人项目，或那个想了多年却一直没开始实现的小应用。与团队分享经验，也向团队学习。一如既往，请继续查阅文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或[云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
