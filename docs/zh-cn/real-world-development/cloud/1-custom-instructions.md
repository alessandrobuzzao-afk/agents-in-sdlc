---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可为 Copilot 提供指导，让它不仅理解你的目标，也了解实现方式。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，让 Copilot 获取项目上下文、编码指南和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会使用前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，你会看到指令文件发挥的作用。

## 指令文件

### 场景

像所有优秀的软件开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 了解这些实践要求，并据此执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。你可以借此引导 Copilot 提供更相关的建议和代码片段，还可以指定首选的编码约定、库以及注释风格。你可以为整个存储库创建指令，也可以为特定任务或文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随该存储库的**每个**请求一起发送给 Copilot。此文件应包含项目级信息，即 Copilot 处理该存储库的大多数聊天或 CLI 请求时都需要了解的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建。你可以使用这些文件为 TypeScript 或 Astro 等语言提供指导，也可以针对创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于如何创建指令文件的详细讨论超出了本实验范围。示例项目展示了一种有代表性的做法，主要原则如下：

- 将 `copilot-instructions.md` 中的指令集中在项目级指导上，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明推荐和不推荐的写法。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目、环境和场景的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览本项目中的文件时，你可能会注意到，其中包含用于[更新 UI][ui-instructions]和 [Astro][astro-instructions] 等任务的指令文件。
>
> Copilot 也可以帮助你生成指令文件。不同产品界面的操作各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。相关界面的课程会在适用时介绍具体操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间查看本存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及用于不同任务的多个 `*.instructions.md` 文件。你可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中简要介绍了项目，并包含 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 中还有一节嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意文件顶部的 `applyTo` 字段，它指定了一个相对于存储库根目录的 glob 模式，用于决定哪些文件适用这些指令。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看创建单元测试时适用的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。通过这种方式，你可以将较大的指令集拆分成较小、可重复使用的文件，并为 Copilot 提供参考示例。（这些路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 一节介绍了项目编码标准，但目前还没有要求在代码中添加文档注释。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要进行唯一一次编辑：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后，云智能体会在本环境中处理你分配的议题时读取这条规则。

开始编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但你仍应先提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来，我们将补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 一节，该节通常位于第 27 行附近。注意其中介绍了项目编码标准，但尚未包含代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件以提供全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如文档注释规则。

## 提交、推送并合并指令更新

云智能体会从议题目标分支创建分支。下一练习中，你会将议题分配给 Copilot，云智能体将从 `main` 分支开始工作。因此，必须先将指令更改合并到 `main`，云智能体才能读取这些指令。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中分配议题后，云智能体便会读取这些指令。

> [!TIP]
> 如果你希望在此研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处使用分支是为了让研讨会流程更贴近真实项目中的工作方式。

## 总结和下一步

你了解了 Copilot 如何从指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一条存储库级标准。接下来的练习中，你将看到 Copilot 云智能体生成的拉取请求如何应用此标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
