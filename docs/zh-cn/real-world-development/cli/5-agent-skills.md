---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能，自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是敲代码。我们已手动验证代码能够运行，也使用说明文件确保代码遵循标准。但测试、lint 和持续集成（CI）的其他部分呢？

这类任务非常适合使用**智能体技能**！技能能帮助 Copilot 了解如何正确执行这些操作。

本课程将：

- 探索现有的 `quality-checks` 技能。
- 自定义结果的格式。
- 重新加载并运行该技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包起来，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含一个名为 `SKILL.md` 的 Markdown 文件。该文件包含 YAML frontmatter，用于定义技能的名称和说明；正文则概述技能的功能，并说明何时应调用它。文件夹中还可以包含脚本和其他资源的子文件夹。

> [!NOTE]
> 技能不一定需要其他文件夹或文件。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

了解 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明会帮助 Copilot 判断何时调用该技能。
3. 阅读其中的说明，了解它如何引导 Copilot 执行测试和 lint。
4. 注意，该技能尚无 **Results output formatting** 部分。

## 修改前先运行技能

技能可通过 Copilot CLI 直接调用，也可以使用自然语言调用。现在请 Copilot 运行该技能中的三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

我们希望报告能说明运行了什么、是否成功，以及工具实际报告了什么。现在更新技能，使其生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，看看效果如何！Copilot CLI 无需重启对话即可重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次的报告进行比较。

## 总结和后续步骤

你已自定义并使用现有的智能体技能。本课程中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果的格式。
- 重新加载并运行了该技能。

这项更改会与筛选功能一起包含在功能 PR 中。接下来，[通过 Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不属于本课程的任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
