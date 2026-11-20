---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的专用 AI 助手。你可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义智能体，为 Copilot 提供有针对性的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理为可重复使用的智能体，例如确保符合 [WCAG][wcag] 要求的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少指定 `name` 和 `description`，后续 Markdown 提示则用于定义智能体的行为、专业知识和指令。

### 自定义智能体与智能体技能的比较

自定义智能体与[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告知 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的人员，技能是它使用的工具。**

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成任务。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与 AI 的其他用法一样，需要在适合你的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不论用户的视觉能力或偏好如何。近期用户反馈指出，由于文字和背景颜色的对比度不足，有些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现。接下来将使用自定义智能体生成此功能。

本练习将介绍如何：

- 审查现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

我们已为你创建了一个无障碍自定义智能体。现在来查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建一个用于无障碍工作的新分支（这样可以将练习 3 中的筛选功能 PR 分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接着浏览并审查以下内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见错误和陷阱。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。你可以选择自定义智能体，让聊天会话应用其指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的会话。每个练习的模式和模型选择可能不同，相关步骤会在适用时说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉列表（其中显示当前智能体，默认情况下为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和差异供你审查。
6. 随着编辑内容加入，**Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改代码库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习中，你将审查并指导正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是为特定任务和领域定制的专用 AI 助手。通过自定义智能体，你可以将团队的专业知识和标准整理为可重复使用的指令，引导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 定义自定义智能体；
- 在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监视并指导智能体的工作][next-lesson]，审查其进行中的更改，并要求它在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监视和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
