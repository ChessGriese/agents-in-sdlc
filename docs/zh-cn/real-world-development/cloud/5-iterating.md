---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

整个实验中，你已与 GitHub Copilot 协作完成了多项改善用户体验和添加功能的任务。你让 Copilot 为代码添加文档、为设计团队构建可迭代的相关游戏功能，并实现高对比度和浅色模式切换等无障碍功能。现在来检查代码更改，必要时向 Copilot 提供反馈以改进结果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在允许团队其他成员开始审查之前，先检查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全性与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保整个过程安全：

- Copilot 对存储库只有读取权限，**仅**对用于编写代码的分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并会创建独立的临时环境。
- 所有 GitHub Actions 工作流都需要人工批准后才能运行。
- [默认情况下会限制对外部资源的访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先检查 GitHub Copilot 云智能体创建的第一个拉取请求（PR）——为代码添加文档。通过 GitHub.com 上常用的 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括调用外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更具体内容的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会显示 **[WIP]** 标志。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一会儿，也可以回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![拉取请求中的 Files changed 选项卡，用于查看 Copilot 修改的文件。](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新添加的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些内容来自练习 2 开始时查看的自定义说明文件。

6. 完成审查并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 应该会看到有些工作流正在等待批准的提示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![拉取请求页面中用于批准并运行等待中工作流的按钮。](../../../_images/shared-approve-workflows.png)

9. 拉取请求的检查区域中应会看到工作流排队运行。如果一切正常，单体 Astro 应用的项目检查会通过。完成检查可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作不是单向的。你可以像标记团队成员一样，在拉取请求评论或代码内联评论中标记 Copilot。Copilot 会读取这些评论，并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。以下是可以要求 Copilot 更新的内容示例：

- 在每个代码文件顶部添加注释标题，简要说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用程序结构。

1. 添加评论请求 Copilot 修改生成的文档，并像标记其他用户一样标记 **@copilot**。可以采用上述任一建议，也可以提出其他希望 Copilot 在代码库文档中处理的内容。
2. 选择 **View Session**，观察 Copilot 如何执行任务。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回拉取请求。

    ![Copilot 会话页面中返回拉取请求的 Back to pull request 按钮。](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到对结果满意。满意后，可将 PR 从草稿状态转换为可审查状态，并合并到主分支。

![GitHub 拉取请求页面中将草稿 PR 转为可审查状态的操作。](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 创建的 PR，检查解决“在游戏详情页中显示相关游戏”议题的更改。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更具体内容的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 完成审查并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 应该会看到有些工作流正在等待批准的提示。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![拉取请求页面中用于批准并运行等待中工作流的按钮。](../../../_images/shared-approve-workflows.png)

8. 拉取请求的检查区域中应会看到工作流排队运行。如果一切正常，单体 Astro 应用的项目检查会通过。完成检查可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以迭代使用 Copilot 云智能体请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或确保添加文件注释标题和 TSDoc 文档注释（请记住——分配此任务时，尚未更新自定义说明！）。与之前一样，可在 **Conversation** 选项卡中添加新评论，Copilot 会读取并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务中心请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更具体内容的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会显示 **[WIP]** 标志。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，特别留意：
   - 用于切换模式的 UI 组件
   - 如何使用本地存储保存用户偏好
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码

6. 完成审查并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 应该会看到有些工作流正在等待批准的提示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![拉取请求页面中用于批准并运行等待中工作流的按钮。](../../../_images/shared-approve-workflows.png)

9. 拉取请求的检查区域中应会看到工作流排队运行。如果一切正常，单体 Astro 应用的项目检查会通过。完成检查可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用并在浏览器中测试高对比度和浅色模式切换，确认它们能正常工作，并且在重新加载页面后仍然保留设置。

注意，无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进机会，可以像处理前面的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习——继续委派任务

云智能体最适合处理实际待办事项，让你有时间专注于其他工作。为养成这种习惯，可以在存储库中创建几个议题并分配给 Copilot。以下是一些想法：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。在这些课程中，你：

- **检查了此存储库附带的自定义说明文件**，以便稍后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它设置环境、规划和异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务中心**，监控并引导进行中的无障碍会话。
- **审查并迭代云智能体的拉取请求**，通过标记 `@copilot` 请求更改，并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作流。如果想继续探索，其他工作流也可以补充本次练习中的内容：

- 🖥️ **[VS Code 工作流](../../vscode/)**——直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)**——从终端交付两个经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，可以尝试以下后续练习：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
