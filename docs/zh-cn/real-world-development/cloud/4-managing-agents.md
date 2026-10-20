---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几次练习中，你要求 Copilot 云智能体执行了三项独立任务，旨在改善用户体验并添加功能。云智能体虽然以异步、自主方式工作，但仍需监控任务。

有许多工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务中心中，可以查看所有拥有开放拉取请求（PR）的智能体任务。你可以探索智能体执行的操作，甚至引导正在进行的会话，帮助它完成工作。

本课程将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到现在也适合添加浅色模式。既然正在更新网站样式并添加切换功能，一并实现浅色模式也合情合理。你希望引导智能体，同时添加浅色模式和高对比度模式。

## 审查 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该会看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，审查 Copilot 已执行的操作。这些操作可能包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和所采用的方法而异。

4. 同时注意右侧显示的拉取请求（PR）面板。你可以在这里查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

了解当前活动任务后，请求 Copilot 在实现高对比度模式的同时添加浅色模式切换。

1. 选择与添加高对比度模式有关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Copilot 智能体任务中心中正在处理无障碍功能的会话。](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始实际工作。看到类似以下消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中的 Steer active session while Copilot is working 对话框。](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 执行任务

与之前一样，Copilot 会根据更新后的任务开始工作！收到新消息时，它会先完成正在执行的步骤，然后再将新请求纳入工作流程。

与之前一样，这会花费几分钟，正好可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课探索了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心控制台。你可以使用任务中心跟踪所有活动和已完成的任务、审查正在执行的工作，甚至重定向进行中的任务以调整范围或提供更多指导。

你了解了以下内容：

- 使用任务中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求增加功能。

Copilot 正在完成无障碍功能，接下来可以[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
