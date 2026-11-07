---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase toda organização enfrente problemas de dívida técnica. Pode ser o acúmulo de problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um rascunho de pull request (PR) para uma pessoa desenvolvedora revisar. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver algumas pendências técnicas. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar jogos relacionados, ou seja, outros títulos da mesma categoria, para que as pessoas possam continuar navegando. Por enquanto, a equipe não precisa de uma implementação refinada; quer algo que permita testar a experiência de usuário. Essa tarefa está bloqueada, mas há issues de prioridade mais alta.

Esses dois exemplos podem acabar ficando para depois e são ótimas tarefas para atribuir ao agente de nuvem. Ele trabalha de forma assíncrona enquanto você se dedica a outras atividades; depois, você volta para revisar o trabalho e confirmar se está correto.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa da equipe, há várias maneiras de atribuir tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um rascunho de pull request para acompanhar o progresso, prepara o ambiente e inicia o trabalho. Você pode examinar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não faz mágica: você ainda precisa pensar. Até operações aparentemente simples podem se tornar complexas ao examiná-las em detalhe. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Considere-o um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções personalizadas, viu o arquivo `.github/copilot-instructions.md` em ação. Caso contrário, aproveite para consultá-lo agora. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos no repositório, sem alterá-los, para depois observar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub ou no codespace, se ele estiver em execução:

- `.github/copilot-instructions.md`: revise a seção **Code standards**, principalmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários. Esses elementos vêm dos arquivos de instruções. Você voltará a examiná-los ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Desenvolver código normalmente exige um ambiente específico e scripts de configuração para instalar ferramentas e dependências. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início da tarefa. Assim, o agente tem acesso às ferramentas e dependências necessárias. Esse workflow já foi configurado para o laboratório. Ele fornece Node.js, dependências do projeto, navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido usado pelo aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para estas etapas. O Copilot recebe seu próprio token para as operações dele.
    permissions:
      # Se as etapas de configuração clonarem o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório, o Copilot fará isso automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora é todo baseado em Astro + Drizzle/libSQL).
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

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro workflow do GitHub, mas há alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se a configuração funciona sem esperar uma tarefa do Copilot.

## Adicionar documentação

Todo mundo entende a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica costuma ficar sem solução, o que reduz a produtividade e dificulta a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação, por isso esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Você voltará a revisar o trabalho em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot prepara o ambiente executando o workflow descrito acima, analisa o projeto e define uma abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request. Você deverá ver uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema: explora o código, entende o estado do projeto, planeja e cria código.

Isso pode levar alguns minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona para que você possa se dedicar a outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, trabalhar em novos recursos do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é poder dividir o trabalho: você se concentra em uma tarefa enquanto ele trabalha em outra. Mesmo que a seção de jogos relacionados não leve muito tempo para ser implementada, esse tempo pode ser usado para outras tarefas. Vamos atribuí-la ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam resolvidas de forma assíncrona. O Copilot pode ajudar a resolver dívida técnica, criar recursos ou migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e pode acessar as instruções personalizadas exploradas nas lições anteriores.

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
