---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu ao agente de nuvem do Copilot três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você vê todas as tarefas do agente com pull requests abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem.
- orientar uma sessão em andamento para pedir uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria útil incluir também um modo claro. Como o trabalho já envolve atualizar os estilos do site e adicionar controles, faz sentido incluir esse recurso. Você orientará o agente para garantir que ele implemente os dois modos.

## Revisar as tarefas do agente de nuvem

Vamos conferir o status atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações já executadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que adicione um modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às descritas na lição.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo Steer active session while Copilot is working destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer a solicitação e incluí-la no trabalho.

## Deixar o Copilot trabalhar

Assim como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso pode levar alguns minutos, então aproveite para refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, o painel central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Com esse recurso, é possível acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar uma sessão em andamento para ajustar o escopo ou fornecer orientações.

Você explorou estes conceitos:

- usar o painel de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
