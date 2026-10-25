---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几次练习中，你已请 Copilot 云智能体处理三个不同任务，以改善用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。你可以在这个任务控制中心查看所有有未关闭拉取请求（PR）的智能体任务、检查已执行的操作，并引导正在进行的会话。

在本练习中，你将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体创建高对比度模式后，团队意识到也可以趁机添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此可以将此功能一并完成。你将引导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务当前状态。

1. 访问 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口（API）。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 实际列出的步骤因 Copilot 的工作状态和采用的方案而异。

4. 同时查看右侧显示的 PR 面板，其中会列出 PR 和更改的文件，方便进一步监控。

## 引导云智能体

现在已看到当前任务，请求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择用于添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

   ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始实际工作。看到类似后续示例的消息时，表示已进入此阶段。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents 页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将它纳入工作流程。

## 等待 Copilot 完成

Copilot 会继续处理更新后的任务。收到消息后，它会先完成当前步骤，再将新请求纳入后续工作。

与之前一样，这需要几分钟。等待期间，可以回顾目前学到和探索的内容。

## 总结和后续步骤

本课探索了 Copilot agents 页面，它是监控和引导 GitHub Copilot 云智能体任务的中心。你可以在此跟踪所有进行中和已完成的任务、审查正在执行的工作，并重定向进行中的任务以调整范围或补充说明。

你探索了以下概念：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向进行中的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
