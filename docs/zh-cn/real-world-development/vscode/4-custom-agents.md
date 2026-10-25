---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域创建的专用人工智能（AI）助手。通过在存储库的 `.github/agents` 文件夹中编写 Markdown 文件，可以为 Copilot 提供有针对性的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识整理为可复用智能体，例如确保符合 [Web Content Accessibility Guidelines（WCAG）][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面则是定义智能体行为、专长和说明的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处：两者主要通过 Markdown 文件定义，并指导 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是它使用的工具**。

自定义智能体有自己的上下文窗口，能够在工作中协调技能，甚至协调其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以调用 PR 工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与其他 AI 功能一样，应在实际环境和场景中进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，部分用户觉得当前深色主题难以阅读，因为文本与背景颜色对比度不足。为解决这一无障碍问题，设计团队要求实现可开关的高对比度模式。

在本练习中，你将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 检查无障碍自定义智能体

项目已为无障碍工作创建了自定义智能体。先检查其内容，了解它如何指导 Copilot。

返回 Codespace，在终端中从 `main` 创建新分支，以便与练习 3 中的筛选 PR 分开处理无障碍工作：

1. 运行以下命令：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

2. 打开 `.github/agents/accessibility.md`。
3. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

4. 浏览并审查后续部分，了解其中的：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，会话将采用该智能体的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 继续操作前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。各练习的模式和模型选择可能不同；需要时会在相应步骤中说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 在聊天视图底部选择智能体下拉列表（默认情况下显示当前活动智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑完成，**Files changed** 指示器会更新，显示智能体修改过的工作文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库，编辑 Astro 组件、CSS 和相关测试等文件。

下一练习中，你将审查并引导这项进行中的工作。

## 总结和后续步骤

本课探索了 GitHub Copilot 中的[自定义智能体][custom-agents]：针对特定任务和领域设计的专用 AI 助手。借助自定义智能体，团队可以把专业知识和标准整理为可复用智能体，从而更有效地指导 Copilot 完成工作。

你了解了以下概念：

- 自定义智能体的定义方式。
- 在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查过程中的更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
