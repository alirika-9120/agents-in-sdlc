---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções, práticas recomendadas, padrões de código e conhecimento de domínio que ajudam o Copilot a realizar determinados tipos de trabalho. As equipes podem registrar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Há certa sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills — e até outros agentes — como parte do trabalho. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Durante essa tarefa, ele poderá chamar skills, como uma skill para o fluxo de pull requests ou para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente da capacidade visual ou das preferências de cada uma. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é fundamental, queremos implementar esse recurso rapidamente. Para isso, usaremos um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado para tarefas de acessibilidade já foi criado. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace, abra um terminal e crie uma branch nova a partir de `main` para o trabalho de acessibilidade. Assim, o PR de filtragem do Exercício 3 permanecerá separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no menu de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar a conversa às instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indicará o que é necessário.

1. No menu de agentes da visualização Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu de agentes na parte inferior da visualização Chat. Ele mostra o agente ativo; por padrão, **default**.
3. Na lista de agentes disponíveis, selecione **Accessibility agent**.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. O indicador **Files changed** será atualizado à medida que o agente alterar arquivos, mostrando o conjunto de trabalho modificado.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, editando componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível registrar os conhecimentos e padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que forem feitas e adicionando um modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
