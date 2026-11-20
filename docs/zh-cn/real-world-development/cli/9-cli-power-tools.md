---
title: "课程 9 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委托到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 包含许多用于交互的斜杠命令。这些命令可以访问高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来，我们来探索其他一些实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文；
- 使用 `/model` 探索可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树以及委托任务给云智能体的可选命令。

## 场景

核心 CLI 工作流程已经完成。现在来了解其他一些功能，包括管理上下文、切换模型、共享会话，以及可选地将工作委托给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理规模较大或复杂的任务时，可能会达到模型上下文窗口的上限。需要时，Copilot CLI 会自动压缩对话；你也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 注意模型、当前 token 使用情况，以及系统指令、工具、消息和剩余空间的上下文分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话内容较少，可能不会有明显变化。

> [!NOTE]
> 上下文窗口接近填满时，Copilot CLI 会自动压缩上下文。如果想自行选择压缩时机，可以使用 `/compact`。如果要切换到无关任务并开始全新的对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型有不同的优势，不同开发人员也有不同偏好。Copilot CLI 允许你列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或者按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员之间互相协作和分享经验，有助于所有人提升 AI 工具的使用水平。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help` 并查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择符合需求的目标，例如 `/share file` 可导出本地 Markdown 文件。
3. 将导出内容发送给其他人或发布之前，先仔细审查。会话导出可能包含提示、响应和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展并行工作或委托任务

核心研讨会内容已经完成。Copilot CLI 还为大型任务提供了以下命令：

- `/fleet` 可以将独立子任务分配给多个子智能体并行执行。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云智能体，由它异步工作并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建其他工作树或远程任务。尝试使用之前，请先创建范围明确的新任务，并按照常规工作流程审查结果。若要进一步了解异步智能体工作，可以继续学习[云智能体研讨会][cloud-workshop]。

## 总结和下一步

使用 Copilot CLI 斜杠命令可以配置 CLI、共享会话并查看其幕后工作方式。本课中，你：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文；
- 使用 `/model` 探索可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树以及委托任务给云智能体的可选命令。

Copilot CLI 还支持更多斜杠命令，也有更多功能值得探索！接下来，我们将[回顾已学内容][next-lesson]并了解后续学习方向，为这段旅程收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
