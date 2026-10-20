---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于上下文、模型、共享以及可选云委派的斜杠命令。

## 你交付的内容

本工作坊包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建：

1. **星级评分：**在游戏卡片上显示已有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并包含相关测试。

从规划筛选功能到创建 PR，你一直使用同一段对话和同一个分支。为简化工作坊流程，这些内容合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：运行自动化测试、亲自在浏览器中查看，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按照新格式报告结果。创建 PR 前，QA 将这些结果与需求和测试覆盖率审查结合起来。

新增测试应弥补实际缺口；QA 运行后发现无需新增测试也可能是正确结果。授权合并前，请审查代码和证据；代码更改后，也应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可在不同对话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复任务，自定义智能体用于定义专用角色，MCP 服务器用于连接外部工具。请审查实际更改和工具结果，而不要只看智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。常规编辑可选较快的模型，复杂工作则可选能力更强的模型。

上下文与基础设施同样重要。清楚描述要构建的*内容*、*原因*和*方式*，会显著影响输出。

## 继续探索

你已完成核心工作流。还有一些 CLI 功能值得了解：

- `/review`：请求代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取另一个视角。
- `/fleet`：并行协调互不相关的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最佳方式就是持续使用它！可以将它用于生产代码、个人项目，或用于实现那个想了多年却一直没时间动手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
