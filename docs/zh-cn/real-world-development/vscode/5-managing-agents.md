---
title: "练习 5 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

启用 GitHub Copilot 智能体模式后，它会自主工作：探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在聊天中查看每个差异、接受或拒绝单项更改，也可以通过后续提示引导对话，改进或扩展已有工作，而无需离开编辑器。

本练习将介绍如何：

- 实时监视无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行期间引导智能体，要求它在高对比度模式之外添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出了一个请求：他们还希望为觉得默认主题太暗的用户添加浅色模式切换开关。你无需重新开始新的对话，而是可以引导当前会话扩展工作内容。

## 监视 Copilot 的进度

Copilot Chat 视图会显示智能体的每个步骤，包括探索的文件、运行的命令和提出的编辑。每次文件编辑都会以内联方式显示在聊天中，并带有差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容并查看每次工具调用。你会看到文件读取、文件编辑以及智能体请求执行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改；
    - 选择 **Undo** 还原更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本会话中修改的完整文件列表。

5. 等待智能体完成高对比度模式的实现。完成后，你应该会看到总结更改内容的最终消息。

## 引导智能体添加浅色模式

高对比度模式已完成，现在可以通过同一对话添加浅色模式切换开关。由于仍在同一聊天会话中，智能体会保留之前工作的上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，查看已经进行的更改，并提出浅色模式的修改方案。
3. 与高对比度模式一样，逐项审查新更改。使用 **Keep** 或 **Undo** 处理每个差异。
4. 如果智能体的做法与你的预期不同，例如它试图以你不喜欢的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送后续提示来说明你的要求。

> [!TIP]
> 使用后续提示指导正在进行的会话，比从头开始更快。你可以要求智能体进行调整（“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个开关，请将浅色模式集成到现有设置面板”），或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查更改文件的完整列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并查看差异。确认其中体现了自定义智能体指导的无障碍模式，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课中，你将发布这些更改。

## 总结和下一步

你使用 Copilot Chat 智能体模式监视并指导正在进行的会话，审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展任务，同时保留现有上下文。

本课介绍了以下概念：

- 实时监视智能体的工作；
- 接受或拒绝单项文件编辑；
- 使用后续提示指导正在进行的会话。

下一课将介绍如何发布无障碍功能并审查本地自定义智能体在本实验中生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
