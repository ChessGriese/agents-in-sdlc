---
title: "练习 3 - 自定义智能体"
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

接下来将探索以下自定义智能体相关内容：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，一些用户觉得当前深色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

由于无障碍访问至关重要，你希望尽快实现该功能。接下来使用自定义智能体生成此功能。

## 审查无障碍自定义智能体

系统已为无障碍访问创建了一个自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 Codespace，审查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中的 YAML frontmatter，包含 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续各部分，了解其中列出的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 创建并分配议题

任务中心是管理环境中所有智能体的中心位置。你可以在此向 Copilot 云智能体分配任务、监控任务，甚至重定向并提供额外指导。现在将高对比度模式任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面中已选中的 Accessibility agent 自定义智能体。](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课探索了 GitHub Copilot 中的[自定义智能体][custom-agents]——针对特定任务和领域量身定制的专用 AI 助手。借助自定义智能体，可以将团队的专业知识和标准整理为可复用的智能体，指导 Copilot 更有效地完成特定类型的工作。

你了解了以下内容：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式，接下来可以前往任务中心，[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
