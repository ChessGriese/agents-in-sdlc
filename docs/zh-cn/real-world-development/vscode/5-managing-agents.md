---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

让 GitHub Copilot 进入智能体模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会在本地实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在聊天中查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，在高对比度模式之外添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出一项需求——他们还希望为觉得默认主题太暗的用户添加浅色模式切换。与其重新开始对话，不如引导现有会话扩展功能。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会以内联差异形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时滚动聊天记录，观察每次工具调用。你会看到它读取和编辑文件，以及请求运行的终端命令。
3. 智能体建议编辑文件时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求许可（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。它应该会显示一条总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一段对话中继续添加浅色模式切换。由于继续使用同一聊天会话，智能体会保留先前工作的上下文，包括修改过的组件、采用的样式方法和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会从当前进度继续，探索已有更改，并提出浅色模式的补充内容。
3. 查看每项新编辑，就像审查高对比度模式更改一样，对每个差异选择 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期（例如以不喜欢的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止，然后发送澄清后的后续消息。

> [!TIP]
> 在进行中的会话中引导智能体，比重新开始更快。可以通过后续提示要求调整（“简化 CSS——使用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式整合到现有设置面板”），或扩展范围（“还要为两个切换开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并查看差异。确认自定义智能体中的无障碍模式已落实，包括 ARIA 属性、键盘导航、语义化 HTML，以及通过本地存储持久保存设置。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat 智能体模式监控并引导进行中的会话——逐一审查生成的文件编辑、接受或拒绝更改，并在保留上下文的同时发送后续提示来扩展工作。

本练习中，你了解了：

- 实时监控智能体的工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示引导进行中的会话。

在[下一课][next-lesson]中，你会将无障碍工作发布为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
