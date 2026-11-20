---
title: "课程 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查其拉取请求，并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问地址。Copilot 可以准备工作流并指导部署，但你仍需负责审查更改并确认网站已上线。

本课将介绍如何：

- 让 Copilot 配置 GitHub Pages 部署；
- 在合并前审查工作流和拉取请求；
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但用户还没有方便的方式来试用。使用 GitHub Pages 发布这个单文件网站后，用户便可以通过公开 URL 访问它，无需添加服务器或运行时依赖项。

## 让 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。发送此提示前，请检查批准设置和目标存储库。如果选择 **Approve all**，系统不会再单独向你确认。

在 Copilot app 中打开 `space-quiz` 会话，并发送以下提示：

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## 审查工作流和拉取请求

1. 确认只有在存储库尚无 README 时才添加 README。
2. 审查拉取请求的更改，包括工作流的构建和部署步骤、触发条件及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 如果需要更正，请提出请求并审查更新后的差异。授权合并前，等待所有必需的拉取请求检查通过。

## 批准合并和部署

> [!WARNING]
> 以下提示授权智能体合并已审查的拉取请求、更改存储库 Pages 设置并发布网站。只有在完成审查并确认存储库和拉取请求后，才发送此提示。根据批准设置，智能体可能无需再次确认便可执行这些操作。

将 `<PR_NUMBER>` 替换为已审查拉取请求的编号，然后单独发送以下提示：

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三个阶段的示意图：合并的 space-quiz 拉取请求；GitHub Actions 中包含 Build website、Upload artifact 和 Deploy to Pages 步骤的成功 Deploy website 运行；以及 yourname.github.io/space-quiz/ 上线的 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 跟踪 GitHub Actions 部署运行，直到成功。
2. 打开报告中的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请遵循它提供的具体说明。只有部署成功且可以正常加载线上网站时，才能将任务视为完成。

## 总结和下一步

Space Quiz 现已作为线上网站发布，你也已亲自验证部署结果，而不是只依赖智能体的报告。继续学习[课程 13：审查与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
