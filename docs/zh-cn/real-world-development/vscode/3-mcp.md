---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认功能在浏览器中能够正常工作。无需自己逐一操作应用，你可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器并测试功能，然后发布分支并创建 PR。

本练习将：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展工具。
- 在工作区中添加 Playwright MCP 服务器。
- 请求 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以连接编辑器之外的资源，从而大幅扩展能力。Copilot 通过 Model Context Protocol (MCP) 实现这一点——MCP 是智能体与外部工具和服务通信的标准方式。

![示意图展示智能体模式的内部工作方式，以及它如何与上下文、LLM 和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，从而使用资源获取最新信息，并使用工具代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理 AI 智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都提供一组智能体可使用的工具和资源。

以下是一些常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让 AI 智能体可以创建新存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可提供不同的工具和资源。GitHub 托管了一个 [MCP 注册表](https://github.com/mcp)，方便开发者发现服务器并为生态系统做出贡献。

> [!CAUTION]
> 出于安全考虑，对待 MCP 服务器时应像对待项目中的其他依赖项一样谨慎。使用前请仔细审查源代码、验证发布者，并考虑安全影响。仅使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 其中应该已配置 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

该 `github` 配置随项目模板提供，因此 Copilot 才能在上一练习中读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，因此无需在本地安装——VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时通过 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供一个可控浏览器，正好适合用来测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 配置，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志表示 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装所需的浏览器。如果 Copilot 随后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选操作——如果跳过，VS Code 会在下一步中 Copilot 首次需要其工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供真实浏览器。无需亲自操作应用，智能体就能打开页面、导航、应用筛选并读取结果，然后向你总结它观察到的内容。无需离开对话，这是快速确认功能是否符合预期的方式。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会根据有标签的结构化元素（按钮、链接、列表项）进行推理，方式与辅助技术类似——因此，快速功能检查也能顺带进行基本的无障碍检查。

连接服务器并启动应用后，请 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步执行测试，并报告结果。请将总结与议题中的验收标准进行比较——如果发现问题，可继续提问，或让它修复代码后再创建拉取请求。

> [!NOTE]
> 此测试要求应用运行在 `http://localhost:4321`。如果已停止开发服务器，请先重新启动再发送提示。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），供团队审查。首先发布 `filtering-vscode` 分支。

1. 打开 Codespace 中的 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![VS Code 的 Source Control 面板，显示 Copilot 修改的文件和提交操作。](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求的方式有多种，包括使用 github.com 和 GitHub 命令行界面（CLI）。不过，既然已在使用 GitHub Copilot，就让它来创建 PR 吧！它可以查找相关议题，并创建关联到该议题的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉菜单中选中了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 请求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你通过 GitHub 进行身份验证——按照提示授权即可。
5. 注意 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接并审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展工具。
- 在工作区中添加了 Playwright MCP 服务器。
- 在发布筛选功能前，使用它在浏览器中手动测试了功能。
- 发布了分支并为筛选工作创建了拉取请求。

你使用 MCP 服务器*测试*了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
