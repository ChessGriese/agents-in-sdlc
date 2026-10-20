---
title: "Lição 5 - Personalizar e usar uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código não é suficiente. Você já verificou manualmente se ele funciona e usou arquivos de instruções para garantir que siga os padrões. E quanto aos testes, ao lint e às outras etapas de integração contínua (CI)?

Para essas tarefas, as **skills de agente** são ideais. Elas ajudam o Copilot a executar operações como essas da forma adequada.

Nesta lição, você vai:

- explorar a skill existente `quality-checks`.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura.

## Instruções, scripts e recursos

Skills de agente agrupam instruções reutilizáveis, scripts executáveis e recursos de apoio carregados sob demanda. Em geral, cada skill é uma pasta com um arquivo Markdown `SKILL.md`, cujo frontmatter define nome e descrição e cujo conteúdo explica o que a skill faz e quando deve ser usada. A pasta pode conter scripts e outros recursos.

> [!NOTE]
> Subpastas e arquivos adicionais são opcionais. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa comandos já existentes no projeto.

Skills podem ficar em `.github/skills`, como recursos compartilhados pela equipe, ou na pasta pessoal `~/.copilot/skills`.

## Explorar a skill

1. No Codespace, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description`; a descrição ajuda o Copilot a decidir quando usar a skill.
3. Revise as instruções para testes e lint.
4. Observe que ainda não há uma seção **Results output formatting**.

## Executar a skill antes da alteração

1. Volte à conversa de filtragem no modo Interactive.
2. Peça ao Copilot que execute as três verificações:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Leia o relatório.

## Personalizar o relatório

Acrescente ao final de `.github/skills/quality-checks/SKILL.md`:

```markdown
## Results output formatting

Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
```

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite `/skills reload`.
2. Envie novamente o mesmo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Leia o novo relatório e compare-o com o anterior.

## Resumo e próximos passos

Você explorou, personalizou, recarregou e executou uma skill para testes unitários, lint e verificações de tipos. Essa alteração acompanhará a filtragem no PR. Em seguida, permita que o Copilot interaja com o site usando o [servidor Playwright MCP][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
