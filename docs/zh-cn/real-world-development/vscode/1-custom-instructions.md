---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解某些背景信息，就应确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中关键的一种是[说明文件][instruction-files]，用于指导 Copilot 生成代码。

本练习将：

- 探索项目特定上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选功能切片提交到 `main`，供 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不完全符合你设定的标准。Copilot 具有非确定性。本练习的重点是观察更新说明后行为的*趋势*变化，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 与其他优秀的开发团队一样，有一套开发实践指南和要求，包括：

- 数据层必须始终有单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一个注释块，说明文件的用途。

通过使用说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践执行任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。借助这项强大功能，可以指定首选编码约定、库，甚至代码注释的编写方式。你可以为整个存储库创建说明，也可以为特定类型的文件提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：单个说明文件，会随存储库的**每个**请求发送给 Copilot。文件应包含项目级信息，例如与大多数 Copilot Chat 或 CLI 请求有关的上下文，包括所用技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建说明文件。你可以为特定语言（例如 TypeScript 或 Astro），或特定任务（例如创建 UI 组件或新的单元测试）提供指南。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 中的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了一种具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 用于项目级指南，例如项目简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，保持指南清晰，并提供代码示例，说明代码应当和不应当是什么样子。

创建说明文件没有唯一的正确方式，正如使用 AI 也没有唯一的正确方式。通过实践，你会找到最适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件集合。浏览本项目的文件时，你会看到许多任务的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供的入口各有不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；与当前界面相关的课程会在适当位置说明。
>
> 需要模板或起点？可以探索 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件——其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，注意其中嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。此处匹配任意 TypeScript 测试文件（例如符合 `**/*.test.ts` 的文件）。
5. 查看项目中专用于创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分为较小、可复用的文件，并为 Copilot 提供可参考的示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释标题的规则。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新的对话。不同练习的模式和模型设置可能不同；相关步骤会在适用的位置说明。

要观察自定义说明的影响，先在当前说明文件不变的情况下发送一个提示。稍后更新说明后，再次发送相同提示并比较结果。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取所需的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以修改文件。

   ![Chat 视图中的 Agent 智能体选择器。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和对应测试。
6. 注意，生成的辅助函数是一个类型化函数，以 `db` 客户端为首个参数，并返回类型化的发行商数组——这是 `.github/instructions/drizzle.instructions.md` 中的数据层约定所要求的（该文件适用于 `src/lib/*.ts`）。
7. 注意，生成的代码**缺少** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，它也可能添加文档注释。这种情况没有问题；重点仍是观察更新说明后的*一致性*改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准补充说明，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中记录了项目编码标准，但尚无代码文档规则，这就是刚才生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以为特定语言、文件类型或任务创建 `*.instructions.md` 文件。刚添加的文档注释规则是项目级标准，适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，生成的文件现在会以类似如下的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，生成的函数现在会包含类似如下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于下一练习会继续使用的真实代码。

## 提交说明更新并推送分支

说明文件和存储库中的其他资源一样，都应使用源代码管理进行管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，就可以创建拉取请求，并将其关联到待办事项中的文档标准议题。也可以手动执行，但 Copilot 可以使用已连接到项目的 GitHub 工具代劳。请 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并它。

> [!NOTE]
> 这是本练习中首次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需设置——Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍此连接（Model Context Protocol）的工作方式。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中了 **Agent**。
3. 请求 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明说明更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求你 **Allow** 它代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. 创建 PR 后，使用以下提示请求 Copilot 将 PR 合并到 `main` 并切回本地 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求你 **Allow** 它代表你执行 GitHub 操作和运行 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在，你已使用 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何读取此项目说明文件中的上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 根据*现有*说明生成的内容。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
