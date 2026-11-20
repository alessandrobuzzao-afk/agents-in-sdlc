---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是编写代码。我们还需要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。这正是 MCP 服务器的用武之地。MCP 服务器可以扩展 Copilot CLI 中内置的功能，为它提供更多工具和服务。

本课将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚未提供 Playwright MCP 服务器，则添加该服务器；
- 让智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，就必须实际与 UI 交互。我们希望让 Copilot 像用户一样使用网站，以进一步自动化更改流程，并更有信心地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时与外部工具和服务通信、访问最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助用户发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用前，请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未提供 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能符合预期。现在我们来使用 Copilot 中的 Playwright MCP 服务器重复测试！

1. 使用以下提示，要求 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确告诉 Copilot 使用 MCP 服务器，因为它一般会自动找到合适的工具。但既然你知道应使用哪个工具，明确指出也没有坏处！这有助于获得更一致的结果，还能节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的步骤，以确认功能是否正常。
3. 阅读报告并确认所有行为都符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，然后向你提供报告。

## 总结和下一步

恭喜！你使用 Playwright MCP 服务器，让 Copilot CLI 在真实浏览器中探索了功能！回顾一下，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚未提供 Playwright MCP 服务器，则添加该服务器；
- 让智能体驱动浏览器并探索筛选功能。

接下来，你将[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [在 Copilot CLI 中添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
