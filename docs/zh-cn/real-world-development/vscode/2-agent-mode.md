---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者的典型工作流程包括：找出所有相关文件、进行更改、运行测试、调试、找出漏改的文件，再次修改……这样的工作还会不断重复。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式更适合在 IDE 中自主执行任务。它的工作方式类似开发者：先探索现有项目结构，再进行必要的更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但用于两种不同的体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队中的同事一样异步工作，通过议题和拉取请求与你协作。

本练习将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调修改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 如何在将更改合并到代码库前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户能够按类别和发行商筛选。上一练习中已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个开发者主题棋盘游戏的众筹平台，允许用户浏览游戏列表和详情。它是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/` 中；交互功能通过使用标准 DOM API 的小型作用域 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，方便运行网站。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应该已回到 `main` 分支。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端窗口。
3. 创建并切换到用于筛选工作的分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示一条提示 URL 的消息，例如：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 的链接会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 Codespace 的私有隧道，Codespace 现在正在其中托管 Web 服务器。

### 探索网站

网站运行后，可以探索其功能。网站的主要功能包括：

- **主页**：显示棋盘游戏列表及其标题、图片和说明。
- **游戏详情页**：选择某款游戏后，会打开详情页，显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新的对话。不同练习的模式和模型设置可能不同；相关步骤会在适用的位置说明。

网站的初始实现已经可以正常运行，但我们还想添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题——请 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的 Agent 智能体选择器。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此在说明中列出了该模型，希望让你获得相对一致的体验。不过，由于 LLM 具有概率性，你收到的建议可能与工作坊所述不同。这完全正常，且符合预期。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 审查生成的议题列表。

注意，Copilot 甚至会根据它认为对用户最有帮助的程度，对议题进行优先级排序。

## 审查说明文件

让智能体生成代码前，先查看用于向 Copilot 提供工作上下文的说明文件。接下来会使用[用户界面 (UI) 说明][ui-instructions]，其中记录了向网站添加功能时应遵循的做法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 阅读整体指南，了解添加功能时应遵循的做法，其中包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向特定文件类型说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用内容、将复杂说明拆分为更易管理的小部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用程序进行三项独立更新：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，添加筛选 UI。

此外，将更改合并到代码库前必须运行并通过测试。Copilot 智能体模式可以完成这些任务！现在添加筛选功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![从 Add Context 菜单中选择 Instructions 和 ui 文件的界面。](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择了 **Agent**。

   ![Chat 视图中的 Agent 智能体选择器。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择了 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，根据待办事项中的相关议题请求 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与功能相关的文件。你会看到它查找数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 智能体模式查看项目文件并分析筛选任务的会话。](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo** 按钮，因此工作进行时无需选择它们。

7. Copilot 提示时，选择 **Continue** 以运行测试。

   ![Copilot Chat 询问是否继续运行项目测试的对话框。](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间迭代，直到完成任务且不再发现错误。

   ![Copilot 智能体模式生成更改并提供文件审查的会话。](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请记住，务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，我们了解了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。我们学习了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调修改。
- 如何在将更改合并到代码库前，审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索练习 - 实现分页

游戏列表不断增长后就需要支持分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。设计代码时可考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑如何实现分页：允许用户选择每页大小，还是使用固定值。
- 编写提示时，提供必要的指导，让 Copilot 按照期望实现功能。
- 可能需要多次迭代，要求 GitHub Copilot 更改实现并提供更多上下文。这是使用 Copilot 时的常见流程！

## 资源

- [Copilot ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
