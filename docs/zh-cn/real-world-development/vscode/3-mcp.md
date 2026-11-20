---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚才使用智能体模式构建了筛选功能。在创建拉取请求之前，应该确认它在浏览器中确实能够正常工作。你无需亲自逐一测试应用程序，而是可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用 Playwright MCP 服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式连接到编辑器之外的资源时会更加强大。Copilot 使用 Model Context Protocol (MCP) 连接外部工具和服务；它是一种智能体与外部工具通信的标准方式。

![智能体模式的内部工作原理示意图，以及它与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）的交互方式](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时与外部工具和服务通信，访问最新信息（使用资源），并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理与外部工具（例如现有 API 或 NPM 包等本地工具）的通信。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建新存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体可以浏览网页、填写表单和选择按钮。

还有许多 MCP 服务器可用于访问其他工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 在安全方面，应像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用 MCP 服务器前，请仔细审查其源代码、验证发布者并考虑安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中包含此 `github` 条目，因此上一练习中的 Copilot 可以读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来，我们将添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 控制浏览器，正好适合用来测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动服务器。`--headless` 标志表示 Playwright 在后台运行浏览器，不显示浏览器窗口。codespace 中没有桌面环境，因此必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器：Copilot 首次需要访问某个服务器的工具时，VS Code 会启动该服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择此操作以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问你是否信任该服务器，请先审查配置，然后选择信任以启动服务器。

手动启动是可选步骤。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要使用 Playwright 工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 驱动真实浏览器。智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察结果，无需你亲自浏览应用程序来检查工作。这是在不离开对话的情况下快速确认功能是否符合预期的方式。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]工作，而不是使用截图。这意味着智能体会像辅助技术一样，根据带有标签的结构化元素（按钮、链接和列表项）进行推理，因此快速功能检查也可以顺便验证基本无障碍性。

连接服务器并启动应用程序后，提示 Copilot 测试刚才实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，完成各项测试，并报告观察结果。根据议题中的验收标准审查其总结。如果发现问题，可以提出后续问题，或让它在创建拉取请求前修复代码。

> [!NOTE]
> 测试时，应用程序必须在 `http://localhost:4321` 上运行。如果已停止开发服务器，请在发送提示前重新启动。如果 Copilot 首次使用 Playwright MCP 服务器，可能需要下载浏览器；如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

你可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。不过，既然已经在使用 GitHub Copilot，不妨让它来创建 PR！它可以找到相关议题，并创建与该议题相关联的 PR。

1. 转到 Copilot Chat 面板并选择 **New Chat**，开始新的会话。
2. 确保从智能体下拉列表中选择了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 提示 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证；按照提示完成身份验证。
5. 注意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接以查看拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！在本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 使用该服务器在发布功能前让 Copilot 在浏览器中手动测试功能；
- 发布分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。下一步，让我们[创建自定义智能体][next-lesson]，简化无障碍审查等特定任务。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
