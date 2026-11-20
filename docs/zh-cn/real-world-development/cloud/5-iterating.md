---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 合作完成了多个任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队创建相关游戏功能以便他们迭代改进，还实现了高对比度和浅色模式切换等无障碍功能。接下来，我们将检查代码更改，并在需要时向 Copilot 提供反馈，帮助它改进工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，我们将在为团队其他成员启用审查前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提供的建议。

## 安全性和 GitHub Copilot 云智能体

由于 Copilot 云智能体会在没有监督的情况下异步执行任务，因此我们设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只能读取存储库，并且仅能写入它用于修改代码的分支。
- 云智能体在 GitHub Actions 中运行，并创建一个单独的临时环境来工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 审查生成的文档

我们先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR）：为代码添加文档。此任务使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到提示，说明 GitHub Copilot 被防火墙阻止。这是预期行为，因为默认情况下 Copilot 只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整标题的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息，或回顾至今学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新添加的 TSDoc 文档注释及其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 确认更新符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会在拉取请求的检查部分看到工作流已加入队列。如果一切正常，单体 Astro 应用程序的项目检查将通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像对待团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果并非确定，因此我们无法提供固定的提示文本。你可以要求 Copilot 修改以下内容：

- 在每个代码文件顶部添加注释头，简要说明文件用途；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用程序的结构。

1. 添加评论并在其中提及 **@copilot**，请求它修改生成的文档。可以使用上面的任意建议，也可以提出其他有关代码库文档的修改要求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意 Copilot 会启动新会话来更新内容。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡查看更改。

如果你还不满意，可以继续迭代。满意后，可以将 PR 从草稿转换为准备好审查，然后合并到 `main` 分支。

![将 PR 转为准备好审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在，我们来查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更完整标题的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到一些工作流正在等待批准的指示器。
7. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会在拉取请求的检查部分看到工作流已加入队列。如果一切正常，单体 Astro 应用程序的项目检查将通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！与之前一样，你可以通过迭代改进 Copilot 云智能体的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令！）。与之前一样，你可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，我们来审查使用无障碍自定义智能体实现的功能。这个 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中通过任务控制中心要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更完整标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，重点检查：
   - 用于切换模式的 UI 组件；
   - 使用本地存储来保留用户偏好的方式；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更新符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会在拉取请求的检查部分看到工作流已加入队列。如果一切正常，单体 Astro 应用程序的项目检查将通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常并且在重新加载页面后仍然生效。

注意，无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或可改进之处，可以像之前一样在评论中提及 **@copilot**，请求它进行更新。

## 可选练习 — 继续委派任务

云智能体最适合处理实际待办任务，让你可以转而处理其他工作。为了养成这种习惯，可以在存储库中创建更多议题并分配给 Copilot。以下是一些建议：

- 在游戏详情页上创建支持者意向表单；
- 在游戏列表页上实现分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体环境中的所有练习。在这些课程中，你：

- **检查了此存储库附带的自定义指令文件**，以便之后查看这些文件对云智能体输出的影响；
- **向 Copilot 云智能体分配议题**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用自定义智能体**，实现无障碍功能并添加高对比度和浅色模式切换开关；
- **将 Copilot 智能体页面用作任务控制中心**，监视和指导无障碍会话；
- **审查并迭代改进 Copilot 的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和下一步

你已完成云智能体环境的练习。如果还想继续探索，其他环境的练习可以补充本课程所学内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接来回连接云智能体，交付两个经过审查的更改。

你还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
