---
title: "练习 3 - 自定义智能体"
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

本练习将介绍如何：

- 定义自定义智能体；
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不论用户的视觉能力或偏好如何。近期用户反馈指出，由于文字和背景颜色的对比度不足，有些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现。接下来将使用自定义智能体生成此功能。

## 审查无障碍自定义智能体

我们已为你创建了一个无障碍自定义智能体。现在来查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后审查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接着浏览并审查以下内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见错误和陷阱。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以在其中将任务分配给 Copilot 云智能体、监视任务、重定向会话，或提供更多指导。现在，我们来将创建高对比度模式的任务分配给 Copilot。

1. 转到存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![突出显示自定义智能体和 Accessibility agent 的云智能体分配界面截图](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务！

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是为特定任务和领域定制的专用 AI 助手。通过自定义智能体，你可以将团队的专业知识和标准整理为可重复使用的指令，引导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 定义自定义智能体；
- 将任务分配给自定义智能体。

Copilot 正在后台实现高对比度模式。接下来，我们将从任务控制中心[监视并指导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中使用自定义智能体前的准备工作][org-custom-agents]
- [在企业中使用自定义智能体前的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监视和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
