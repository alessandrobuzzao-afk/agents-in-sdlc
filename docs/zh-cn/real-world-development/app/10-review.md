---
title: "课程 10 - 总结和后续步骤"
description: "回顾 App 工作流、两个 PR 里程碑、画布练习和可重复使用的质量实践，然后探索更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流程中使用了 GitHub Copilot app。你：

- 连接了存储库，探索了应用工作区和模板创建的待办事项，并尝试了快速聊天；
- 启动了范围明确的星级评分会话，在浏览器画布中审查结果，并手动合并了第一个拉取请求 (PR)；
- 从筛选功能议题开始，在 **Plan** 模式中确定方案，在 **Autopilot** 模式中构建功能，然后在 **Interactive** 模式中审查结果；
- 使用自定义指令引导智能体，自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并使用它在真实浏览器中探索筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，评估要求、覆盖率、技能结果和浏览器证据；
- 审查了完整的筛选功能更改，并为第二个 PR 授权 **Agent Merge**；
- 使用现有的 Database Explorer 画布，再创建并测试由存储库支持的分类画布。

## 交付的内容

本研讨会包含两个 PR 里程碑，每个 PR 都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流程：**实现筛选功能、更新指令并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个会话、工作树和分支。为了简化研讨会流程，我们将这些工作合并到一个 PR 中。之后，你使用了现有的 Database Explorer，并创建了一个由存储库支持的分类画布，无需重复 PR 工作流程。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。创建 PR 前，QA 将这些结果与要求和测试覆盖率审查结合起来。

新增的测试应弥补实际缺口。如果 QA 运行发现无需添加测试，也可能是正确结果。缺少工具、跳过检查和失败都属于明确的阻塞项，不能算通过。授权合并前，请审查代码和证据；如果代码发生更改，应重新验证受影响的证据。

## 最佳实践

你提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置、配置了 MCP 服务器并创建了画布。可以在不同会话中重复使用这些自定义设置，并根据团队需求调整。指令定义标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具，画布提供共享的交互界面。应审查实际更改和工具结果，而不仅仅是智能体的总结。

**模式和模型**应与任务相匹配。使用 **Plan** 在构建前确定方案，使用 **Interactive** 对专注于特定内容的更改保持控制，并仅对范围明确且相互独立的任务使用 **Autopilot**。常规编辑可选择速度较快的模型；复杂工作则选择能力更强的模型，并提高推理力度。

上下文与基础设施同样重要。清楚说明要构建*什么*、*为什么*构建，以及*如何*构建，会显著影响输出。快速聊天适合在开始完整会话前明确想法的范围。

## 继续探索

你已经完成核心工作流程。还可以了解一些其他功能：

- 使用 [**Automations**][using-automations] 执行重复性或按需任务，例如总结近期工作。采用前先审查计划、权限和范围；创建自动化任务属于后续方向，不是本研讨会的一部分。
- 使用 **Rubber duck** 分析问题，并在开始构建前获得高信噪比的反馈。
- 使用 [`/chronicle`][chronicle] 生成为会话过程提供说明的记录。
- 使用 [Bring your own key (BYOK)][byok]，从自己的提供商处选择模型，包括通过 Ollama、Foundry Local 或 LM Studio 使用本地模型。
- 使用 [Deep links][deep-links] 在 app 中直接打开存储库、会话或提示。

## 后续步骤

熟练使用任何工具的最佳方式都是持续使用。可将它用于生产代码、业余项目，或者那个想了很多年却始终没有动手构建的小应用。与团队分享经验，也向团队学习。并且一如既往地探索文档。

要探索 GitHub Copilot 生态系统的更多内容，请查看 [VS Code 学习路径][vscode-harness]、[Copilot CLI 学习路径][cli-harness]或 [Cloud agent 学习路径][cloud-harness]。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [自定义 GitHub Copilot app][customize]
- [使用自动化][using-automations]
- [使用画布扩展][canvas-docs]

[vscode-harness]: ../../vscode/
[cli-harness]: ../../cli/
[cloud-harness]: ../../cloud/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[customize]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
[using-automations]: https://docs.github.com/copilot/how-tos/github-copilot-app/using-automations
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[chronicle]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/chronicle
[byok]: https://docs.github.com/copilot/how-tos/github-copilot-app/use-byok-models
[deep-links]: https://docs.github.com/copilot/how-tos/github-copilot-app/open-with-deep-links
