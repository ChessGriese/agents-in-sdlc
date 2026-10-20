---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解某些背景信息，就应确保它能获取这些上下文。[说明文件][instruction-files]用于提供指导，让 Copilot 不仅了解你想让它做*什么*，还了解你希望它*如何*完成任务。

本练习将：

- 探索项目特定上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，本练习不会对比更新说明前后的结果——Copilot 云智能体会在 GitHub 议题上异步工作，因此较难实时并排展示影响。稍后审查云智能体生成的拉取请求时，你会看到说明文件的作用。

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

花些时间阅读此存储库附带的说明文件——其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在 GitHub 网页界面中打开这些文件，也可以在已启动的 Codespace 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，注意其中嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。此处匹配任意 TypeScript 测试文件（例如符合 `**/*.test.ts` 的文件）。
5. 查看项目中专用于创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分为较小、可复用的文件，并为 Copilot 提供可参考的示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释标题的规则。

## 添加新的存储库标准

接下来是本练习中唯一需要编辑的内容：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释并添加文件级注释标题。之后在本工作流中分配议题时，云智能体会应用这项标准。

编辑前先创建一个分支（云智能体会从议题目标分支读取说明，但仍建议清晰地提交编辑）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准补充说明，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中记录了项目编码标准，但尚无代码文档规则，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以为特定语言、文件类型或任务创建 `*.instructions.md` 文件。刚添加的文档注释规则是项目级标准，适合放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中，将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中处理分配的议题时，云智能体就会读取这些说明。

> [!TIP]
> 如果想在本工作坊中直接操作 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了让工作坊流程符合真实项目的工作方式。

## 总结和后续步骤

你已探索 Copilot 如何读取此项目说明文件中的上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。稍后审查云智能体生成的 PR 时，会看到该标准的应用效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
