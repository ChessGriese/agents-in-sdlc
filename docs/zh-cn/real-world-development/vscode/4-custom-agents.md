---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域量身定制的专用 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，从而更有效地完成特定类型的工作。团队可以将专业知识整理为可复用智能体——例如，确保遵循 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局存储在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面跟着定义智能体行为、专业知识和说明的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体与[智能体技能][agent-skills-concept]有一些相似之处。两者主要通过 Markdown 文件定义，并向 AI 说明如何执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**则是工具。

自定义智能体拥有自己的上下文窗口，并且能够在工作过程中协调技能（甚至其他智能体）。在本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站；执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方式。与 AI 的其他应用一样，需要通过测试和迭代找到适合自己环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，一些用户觉得当前深色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

由于无障碍访问至关重要，你希望尽快实现该功能。接下来使用自定义智能体生成此功能。

本练习将：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

系统已为无障碍访问创建了一个自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建一个新分支以开展无障碍工作（让练习 3 的筛选 PR 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中的 YAML frontmatter，包含 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续各部分，了解其中列出的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中列出 `.github/agents` 中定义的所有自定义智能体。你可以选择自定义智能体，让会话使用该智能体的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新的对话。不同练习的模式和模型设置可能不同；相关步骤会在适用的位置说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（其中显示当前活动智能体；默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作——探索代码库、提出更改并应用到项目文件。每项编辑都会在聊天中以内联方式显示，包括文件路径和差异，方便审查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示本次会话中已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习将审查并引导这项进行中的工作。

## 总结和后续步骤

本课探索了 GitHub Copilot 中的[自定义智能体][custom-agents]——针对特定任务和领域量身定制的专用 AI 助手。借助自定义智能体，可以将团队的专业知识和标准整理为可复用的智能体，指导 Copilot 更有效地完成特定类型的工作。

你了解了以下内容：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体的工作][next-lesson]，在更改生成时进行审查，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
