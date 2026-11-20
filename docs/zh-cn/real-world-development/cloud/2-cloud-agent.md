---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置在待办列表中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解明确界定议题范围的重要性；
- 分配议题。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，你会发现项目完全缺少文档。首先，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现功能。他们希望每个游戏的详情页都显示相关游戏，也就是同一类别中的其他游戏。暂时不需要完善的实现，只需要一个可用于验收测试 UX 的版本。目前这项工作已经成为阻碍，但还有优先级更高的议题需要处理。

这两项任务都可能很快被搁置，因此很适合分配给 Copilot 云智能体。云智能体会在后台异步处理任务，让开发人员可以转去处理其他工作，之后再回来审查 Copilot 的成果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过多种方式向它分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话进行中或结束后查看会话。建议方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非万能，你仍需要思考任务。即使看似简单的操作，深入分析后也会发现其复杂性，因此请留意[如何界定 Copilot 云智能体任务的范围][cloud-agent-best-practices]。将它视为 AI 配对程序员：分阶段工作、学习、试验并根据情况调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

之前的练习介绍了自定义指令及其对 Copilot 的引导作用。如果你已完成自定义指令实践，就见过 `.github/copilot-instructions.md` 的效果；如果还没看过，现在正是快速浏览的好时机。将工作分配给 Copilot 云智能体之前，先以只读方式查看本存储库中的指令文件，稍后便能发现它们的影响。

在 GitHub Web 界面或正在运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 一节，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件，稍后的练习还会再次检查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境准备就绪。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样执行任务。

云智能体使用 [GitHub Actions][github-actions] 提供工作环境。你可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在智能体开始工作前运行，以便智能体访问所需的开发工具和依赖项。为便于完成实验，本项目已预先配置此工作流。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用程序所需的已迁移和播种的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此工作流与其他 GitHub 工作流类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否成功，而不必等待 Copilot 运行它。

## 添加文档

每个人都知道文档的重要性，但大多数项目都存在文档过时或完全缺失的问题。这类技术债务常常无人处理，会降低工作效率，也会增加维护代码库或让新开发人员加入团队的难度。Copilot 擅长创建文档，因此很适合将这类议题分配给 Copilot 云智能体。它会在后台生成所需文档。之后的练习中，你会回来审查它的工作。

1. 在新浏览器选项卡中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，你应该会看到 Copilot 创建的待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它需要先创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和 Copilot 即将完成的任务。
14. 向下滚动拉取请求时间线，你应该会看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定合适的计划，以及编写代码。

此过程可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让你腾出时间处理其他工作。接下来，我们会利用这一点：先分配另一个任务给 Copilot 云智能体，然后自己编写代码为应用添加功能。

## 在游戏详情页中添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏部分不一定需要很长时间，仍可以将这段时间用于其他工作。现在就把任务分配给 Copilot 云智能体吧！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二个请求！Copilot 云智能体会像软件工程师一样工作，因此无需一直监视它；等任务完成后再审查即可。现在，让我们开始创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot，让它异步执行任务。你可以使用 Copilot 处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 明确界定议题范围的重要性；
- 分配议题。

Copilot 云智能体正在后台工作，我们现在可以开始创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并支持我们在前面模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
