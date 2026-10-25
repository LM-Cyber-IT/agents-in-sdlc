---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在本机实时执行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以查看每个内联差异、接受或拒绝单项更改，并通过后续提示引导对话，以完善或扩展工作，无需离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体，同时添加浅色模式切换和高对比度模式。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了新要求：还要添加浅色模式切换，供觉得默认主题过暗的用户使用。无需从头开始新对话，你将引导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会在聊天中显示差异以及 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时滚动聊天，查看各项工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 智能体建议编辑文件时，差异会内嵌显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新处理。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。完成后会显示一条总结更改内容的消息。

## 引导智能体添加浅色模式

高对比度模式实现后，在同一对话中继续添加浅色模式切换。由于使用同一聊天会话，智能体会保留之前工作的上下文，包括修改过的组件、采用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会继续工作，探索现有更改并提出浅色模式修改。
3. 新编辑出现时逐项审查，像处理高对比度更改一样使用 **Keep** 或 **Undo**。
4. 如果智能体的处理方向不符合预期，例如尝试以不合意的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止操作，再发送补充说明。

> [!TIP]
> 引导正在进行的会话比重新开始更快。可以使用后续提示调整方向，例如“简化 CSS，使用 CSS 变量而不是重复选择器”；纠正方案，例如“不要添加第三个切换项，将浅色模式整合到现有设置面板”；或扩展范围，例如“同时为两个切换项添加键盘快捷键”。

## 审查修改文件

提交前快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有更改文件。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开部分更改文件并检查差异，确认智能体体现了无障碍做法，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat 智能体模式监控并引导进行中的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示在保留上下文的情况下扩展工作。

在本练习中，你：

- 实时监控智能体的工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示引导正在进行的会话。

下一课将发布无障碍功能并创建拉取请求，同时审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
