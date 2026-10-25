---
title: "第 4 课 - 使用自定义说明指导 Copilot"
description: "探索存储库说明，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果某项任务需要按照特定方式完成，就应让 Copilot 能够获取相应指导。[说明文件][instruction-files]不仅描述你希望生成什么代码，也说明代码应如何组织。完成筛选功能后，现在来探索 Copilot 使用的说明，添加文档标准，并将其应用于代码。

在本课中，你将：

- 探索存储库说明和按路径限定的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TypeScript 文档注释（TSDoc）或 JavaScript 文档注释（JSDoc）说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 保留现有格式和 lint 指南。

通过说明文件，你可以确保 Copilot 遵循这些实践来完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解你的代码风格和要求。你可以指定编码约定、库，以及需要添加的注释类型。说明既可以适用于整个存储库，也可以只针对特定文件或任务。

说明文件分为两种：

- `.github/copilot-instructions.md`：针对该存储库每个请求发送给 Copilot 的单个说明文件。应包含与大多数聊天或 CLI 请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定编程语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他说明格式的支持各不相同。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

启动项目时已附带一组说明文件。在修改前先探索现有文件，了解它们的作用。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指导。这些说明适用于在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含 Astro、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个 glob，用来确定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明拆分成可复用的小文件，并为 Copilot 提供示例参考。

## 更新说明以符合团队规范

现有文件虽然提供了良好基础，但仍缺少一项要求。修改核心 `copilot-instructions.md` 文件，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 将以下内容作为最后一条项目添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存说明后，退出并恢复筛选功能对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，阅读更改的 TypeScript 文件。注意新增的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已探索 Copilot CLI 如何获取说明文件中的上下文，并将新标准应用于功能。具体来说，你：

- 探索了存储库说明和按路径限定的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 了解了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
