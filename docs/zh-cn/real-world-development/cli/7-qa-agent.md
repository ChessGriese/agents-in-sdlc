---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 整合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，也使用 Playwright MCP 在浏览器中观察筛选体验。现在，将这些能力整合到自定义智能体中，并为它定义清晰的 QA 流程。

本课程将：

- 探索自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其结果。

## 场景

创建拉取请求（PR）前，Tailspin Toys 希望始终如一地审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述智能体的用途、说明和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用过的自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能打包可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力评估需求并报告结果。它不会取代其他功能，也不需要另开对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。配置将定义 QA 要执行的检查以及必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，请先检查其配置，确认 Copilot 捕捉到了预期的 QA 流程和边界。这样可避免智能体在你只想验证功能时进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必填项；`name` 可选，但加入后能让智能体显示清晰的名称。
3. 阅读配置说明，确认 QA 会从需求入手、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据、在修改实现代码前征求许可，并且不会提交更改或创建拉取请求。
5. 如果生成的配置缺少上述职责或边界，请要求默认智能体修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置后，恢复同一段筛选对话，再选择 QA，使其能够使用已有上下文中的议题和规划决策。

1. 使用以下提示启用该智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于你刚创建了智能体，它可能不会出现在可用智能体列表中。不过它已存在，上述命令可以将其激活。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作！
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已为工作流添加可复用的专用角色，并审查了它的结果。本课程中，你：

- 探索了自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其结果。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好接受审查。接下来，[整合这些内容并创建功能 PR，然后使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
