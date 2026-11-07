---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo. Você pode revisar cada diff no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz outro pedido: adicionar também um modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para incluir esse recurso.

## Acompanhar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos que ele explora, os comandos que executa e as edições que propõe. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e observe as chamadas de ferramentas. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando do terminal, o agente será pausado e mostrará uma solicitação **Allow**. Aprove somente os comandos que você considerar seguros.

5. Aguarde até que o agente termine de implementar o modo de alto contraste. Uma mensagem final resumirá as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você ampliará a mesma conversa para adicionar um modo claro. Como continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o uso do armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo as adições do modo claro.
3. Revise cada nova edição à medida que aparecer, usando **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de modo de uma maneira inadequada, interrompa-o com o botão **Stop** ao lado do campo de entrada. Depois, envie um prompt complementar para esclarecer o que você quer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts complementares para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações atual") ou ampliar o escopo ("adicione também atalhos de teclado para os dois controles").

## Revisar o conjunto de trabalho

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se a implementação segue os padrões de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento, revisando cada edição, aceitando ou rejeitando alterações e enviando prompts complementares sem perder o contexto.

Neste exercício, você:

- monitorou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão durante o trabalho com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
