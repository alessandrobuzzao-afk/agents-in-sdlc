---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，你通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次修改……这样的工作会一直重复。

Copilot 智能体模式可以帮助你处理这些工作。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来，我们将了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们适用于不同的使用场景。智能体模式在 IDE 中执行任务，可快速反馈并与用户交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和 publisher 筛选游戏。上一练习中已添加 publisher 辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改之前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发人员主题的桌游众筹平台，可以列出游戏并显示其详情。它是一个单体 Astro 应用程序，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可重复使用的数据访问辅助函数位于 `src/lib/`，交互功能则使用小型、限定范围的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于更轻松地运行单体 Astro 开发服务器。请按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已经切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端的 localhost URL 时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

启动网站后，可以浏览它的功能。网站主要包含以下功能：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详情页**：选择某个游戏后，会打开详情页并显示更多信息，包括名称、说明、publisher 和类别。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的会话。每个练习的模式和模型选择可能不同，相关步骤会在适用时说明。

网站的初始实现已经可以正常工作，但我们希望添加新功能。先查看待办列表。你从模板创建项目时，系统已自动创建了一组议题；现在让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未推荐任何特定模型。编写本研讨会时，我们使用了 Claude Sonnet 4.5，因此在说明中列出了该模型，希望由此得到相对一致的代码建议，确保良好的学习体验。不过，LLM 的输出具有概率性，因此你收到的建议可能与本研讨会的说明不同。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用其他 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. 选择 **Continue** 以运行命令并列出所有议题。
5. 查看生成的议题列表。

注意，Copilot 还根据它认为对用户最有用的内容为这些议题排定了优先级。

## 审查指令文件

让智能体生成代码之前，先查看将用于向 Copilot 提供上下文的指令文件。我们将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中转到 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，了解如何添加功能，其中包括：
   - 应用程序架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向各类文件专用指令（包括 Astro、Tailwind CSS 和 Drizzle）的链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置，因此可以重复使用指导、将较大的指导拆分为更易于管理的文件，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用程序进行三项更改：

- 在数据层 (`src/lib/`) 中添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面以加入筛选 UI。

此外，必须在合并到代码库前运行并通过测试。Copilot 智能体模式可以完成这些任务！现在来添加筛选功能。

1. 可以继续当前 Copilot 会话，也可以选择 **New Chat** 开始新会话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![显示如何选择 UI 指令文件的截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍然选择了 **Agent**。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，要求 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 开始探索项目并查找与目标功能相关的文件。你应该会看到它查找数据层辅助函数、UI 和测试，随后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。具体耗时取决于任务复杂度和代码库规模，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作过程中选择这些按钮。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 窗格中的对话框询问用户是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能会看到测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未检测到错误。

   ![完整的 Copilot 智能体模式聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，我们将使用 [Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 奖励探索练习 — 实现分页

随着游戏列表不断增长，需要实现分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。请考虑以下事项：

- 遵循现有最佳实践，包括使用现有指令文件；
- 考虑分页的实现方式，例如是否允许用户选择页面大小，还是固定页面大小；
- 编写提示时，提供必要的指导，让 Copilot 按照你的预期实现功能；
- 可能需要与 GitHub Copilot 迭代沟通、提出更改请求并提供上下文。这是使用 Copilot 的常规流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
