---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 协商实现方案，使用 Autopilot 根据议题构建筛选功能，再使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一项小功能，但较大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的开发流程，帮助团队以正确的方式构建正确的功能。本课将开启一系列课程，带你体验典型的智能体驱动开发流程：从议题出发生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作流将继续使用同一段对话和分支。通常会为不同类型的文件使用不同分支或 PR；这里为了聚焦核心概念而简化了流程。

本课中，你将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。积压工作中的议题描述了这项功能，但组合类别等细节需要在编写代码前达成一致。你将使用 Plan 模式确定这些细节，然后授权 Autopilot 在明确范围内实现功能。

## 背景

引入人工智能（AI）编码智能体并不会改变软件开发的基本原则，反而会让这些原则更加重要。大多数开发人员遵循的流程大致如下：

1. 从已提交的议题开始，了解待完成的工作。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述步骤类似。

遵循这种标准流程，可以确保 AI 生成的代码符合需求，并像人工编写的代码一样经过审核。

## 对话模式

**对话模式**决定智能体拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先制定计划，在计划阶段无法编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并反复迭代，直到认为任务已完成。

先在 Plan 模式中审查计划，然后切换到 Autopilot 实施计划。

## 从议题开始

开始筛选功能之前，返回 Codespace，确认存储库和终端均已就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

在将任务交给 Copilot 之前，先规划功能的实现方式，可以帮助你明确预期。对于复杂工作，花些时间规划通常很有帮助。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定合适的实现方案。

3. Copilot 可能会询问筛选功能的具体行为。根据你的偏好回答，没有唯一正确的答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现了。让 Copilot 使用 Autopilot 执行任务。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实施，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

生成代码后，像合并其他代码一样，先审查代码并运行网站，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 根据议题和规划时确定的行为审查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出；如果有失败，请让 Copilot 修复。

## 体验新功能

代码看起来没问题，但实际运行是否正常？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别和发行商筛选，以及在计划中约定的筛选组合。
4. 确认重置和无结果时的行为符合议题要求和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并审查功能。在本课中，你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，继续了解代码的生成方式，并通过[自定义说明][next-lesson]确保代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
