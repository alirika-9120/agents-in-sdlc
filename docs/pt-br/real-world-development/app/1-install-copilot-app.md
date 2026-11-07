---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele é baseado no GitHub Copilot CLI e se integra nativamente ao GitHub, para que seus repositórios, branches e pipelines de CI funcionem sem configuração adicional. O aplicativo foi projetado para fluxos de trabalho nos quais você orienta vários agentes em paralelo — cada um em seu próprio espaço de trabalho isolado — em vez de fazer todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo a partir do repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para lidar com um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e integrar pull requests. Nesta lição, você instalará e conectará o aplicativo e se familiarizará com o início de uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política do **aplicativo GitHub Copilot**.

## Instalar e configurar o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, a primeira etapa é instalá-lo. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar sua conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo após a instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, escolha **Use GitHub Enterprise** e informe seu nome de host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo de integração oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode pular essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua o processo de integração.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme se o repositório aparece na barra lateral. No layout usado aqui, pastas de repositórios como `tailspin-toys` ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conhecer o espaço de trabalho

Com o projeto conectado, reserve um momento para conhecer o espaço de trabalho. O aplicativo organiza tudo em algumas áreas na barra lateral:

- **New** — permite iniciar uma nova sessão de chat com o Copilot.
- **Pull requests** — mostra seus pull requests por meio da integração nativa com o GitHub. Nessa área, você pode navegar e filtrar pull requests, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues** — mostra suas issues da mesma forma. Nessa área, você pode navegar e filtrar issues e iniciar uma sessão a partir de uma issue.
- **Automations** — tarefas de agente salvas, executadas em uma agenda ou sob demanda. Elas são úteis para gerenciar listas de tarefas, fazer a manutenção regular do projeto ou delegar outras atividades repetitivas. O resumo apresenta links para automações como próximo passo, não como outro exercício do workshop.
- **Customize** — permite adicionar recursos e funções ao aplicativo Copilot na forma de servidores MCP, plugins, skills e outros componentes. Você usará essa área para configurar o MCP do Playwright.
- **Chats** — conversas rápidas para perguntas e brainstorming que não precisam de uma branch ou de um espaço de trabalho próprio. Esta seção aparece depois que você inicia seu primeiro chat. Se não estiver visível, selecione **+** ao lado de **Sessions** para começar um chat.
- **Sessions** — agrupa pastas de repositórios e suas sessões de agente. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a visualização dele, ou selecione uma sessão abaixo do repositório para voltar à conversa. Cada sessão é executada em seu próprio espaço de trabalho isolado, permitindo trabalhar em várias ao mesmo tempo sem conflito entre alterações. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho!

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, pergunte ao Copilot. Ele ajudará você.

### Localizar o backlog criado pelo modelo

É provável que quase todo projeto tenha um backlog, e a Tailspin Toys não é diferente. Vamos explorar o backlog criado quando você fez sua cópia do modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual abaixo dela. Depois, selecione **Issues** na navegação superior do repositório.
2. Encontre estas issues pelo título, em vez de presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também é um ponto de partida para uma sessão de agente. Depois de concluir uma primeira alteração rápida, você começará pela issue de filtragem.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Quer ver itens de trabalho de outros repositórios? Adicione-os ao aplicativo.

## Experimentar um chat rápido

Uma ótima maneira de se familiarizar com o aplicativo é usá-lo para conhecer o *próprio aplicativo* — e um **chat rápido** é a ferramenta ideal para isso. Os chats rápidos permitem fazer perguntas ou trocar ideias sem criar uma branch ou worktree, por isso são perfeitos para uma pergunta breve e descartável, sem precisar de uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois disso.
2. Pergunte ao aplicativo como funcionam as sessões:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em seu próprio git worktree isolado — o que permite trabalhar com vários agentes em paralelo sem conflito entre as alterações. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto a partir do repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog criado em **Issues**.
- usar um chat rápido para fazer uma pergunta breve e descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma avaliação por estrelas nos cards dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
