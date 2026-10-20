---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se é que alguma — não enfrentem dificuldades com dívida técnica. Ela pode se manifestar como problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que permanecem no backlog porque simplesmente não houve tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para realizar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR de rascunho para que um desenvolvedor possa revisar. Isso permite delegar tarefas tediosas, acelerar o processo de desenvolvimento e liberar as pessoas desenvolvedoras para se concentrarem em questões mais abrangentes.

Você vai explorar os seguintes aspectos do agente de nuvem do Copilot:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está totalmente ausente. Como primeiro passo, a equipe gostaria que fossem adicionados comentários TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Ainda não é necessário ter uma implementação refinada; basta algo que possa ser usado para testes de aceitação da experiência do usuário. Isso está bloqueando o trabalho no momento, mas há outras issues que têm prioridade maior.

Esses são dois exemplos de tarefas que podem acabar perdendo prioridade rapidamente e que são ótimas candidatas para atribuição ao agente de nuvem do Copilot. O agente pode trabalhar nelas de forma assíncrona, permitindo que a pessoa desenvolvedora se concentre em outras tarefas e depois volte para revisar o trabalho do Copilot e confirmar se tudo está como esperado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode realizar tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa desenvolvedora, há várias maneiras de iniciar uma tarefa, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode acompanhar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request!

## A importância de instruções com escopo bem definido

Não existe mágica no GitHub Copilot — você não pode deixar de pensar. Até operações aparentemente simples se tornam complexas quando você examina todos os detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a adição da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem do Copilot, examine somente para leitura os arquivos de instruções já incluídos neste repositório para que você possa observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface web do GitHub para seu repositório ou em um codespace, caso já tenha iniciado um:

- `.github/copilot-instructions.md` — Revise a seção **Code standards**, especialmente as expectativas relativas às convenções TypeScript e aos comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — Observe o frontmatter `applyTo`, que limita o escopo dessas instruções aos arquivos `**/*.test.ts`.

Quando atribuir ao agente de nuvem a issue `Code lacks documentation` na próxima seção, observe no pull request resultante os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentário. Essas orientações vêm desses arquivos de instruções. Voltaremos a isso ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código, independentemente de quem esteja envolvido, normalmente requer um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em boas condições. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow especial de configuração][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, para ser executado antes de o agente começar. Assim, ele pode acessar as ferramentas de desenvolvimento e dependências necessárias. Esse workflow já foi configurado antes do laboratório para facilitar o percurso e proporcionar essa oportunidade de aprendizado. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece com qualquer outro arquivo de workflow do GitHub, mas há alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes que o Copilot comece a trabalhar no pull request.
- Observe o acionador `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do seu repositório. Isso é útil para testar se o workflow é executado com sucesso, em vez de esperar que o Copilot o execute.

## Adicionar documentação

Todo mundo entende a importância da documentação, mas a maioria dos projetos tem informações desatualizadas ou não tem documentação alguma. Esse tipo de dívida técnica muitas vezes fica sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas desenvolvedoras à equipe. Felizmente, o Copilot se destaca na criação de documentação, e esta é uma issue perfeita para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se um novo PR não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, você deverá ver que o Copilot criou uma lista de tarefas.

> [!NOTE]
> Pode levar vários minutos para a lista de tarefas do Copilot aparecer no PR. O Copilot está criando o ambiente (executando o workflow destacado anteriormente), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão concluídas.
14. Role a linha do tempo do pull request para baixo. Você deverá encontrar uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot está resolvendo o problema. Isso inclui explorar o código e entender o estado do projeto, observar como o Copilot pausa para refletir e decidir a abordagem adequada e acompanhar a criação de código.

Isso provavelmente levará vários minutos. Um dos principais objetivos do agente de nuvem do Copilot é permitir que ele realize tarefas de forma assíncrona, liberando nosso tempo para outras atividades. Vamos aproveitar justamente esse recurso: além de atribuir outra tarefa ao agente de nuvem do Copilot, vamos nos dedicar a escrever código para adicionar funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Como já mencionamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto o agente cuida de outro. Talvez adicionar uma seção de jogos relacionados para a equipe de design não demore muito, mas esse tempo ainda poderia ser usado em outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como: `Show related games on the game details page`
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora, o Copilot está trabalhando diligentemente na sua segunda solicitação! O agente de nuvem do Copilot trabalha de forma semelhante a uma pessoa engenheira de software, então não é necessário monitorá-lo ativamente; basta revisar o trabalho quando estiver concluído. Vamos nos concentrar agora em criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, é possível atribuir issues ao Copilot para que ele as resolva de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar na migração de código de um framework para outro.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, agora podemos nos concentrar em criar e usar agentes personalizados. [O agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
