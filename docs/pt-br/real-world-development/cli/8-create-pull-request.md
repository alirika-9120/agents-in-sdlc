---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a skill, o perfil de QA e os testes; crie o PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisar tudo em conjunto e abrir um pull request. Você integrou o PR de avaliações por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações de instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma conversa e branch.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar a alteração completa e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente após a revisão e confirmar que o PR foi integrado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle da pessoa desenvolvedora.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request. Quando você o habilita, o Copilot resolve o que estiver bloqueando o PR — por exemplo, verificações de integração contínua (CI) com falha, comentários de revisão ou a necessidade de rebase — e ativa o auto-merge do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado, vamos revisá-lo em conjunto, criar o PR e permitir que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua no PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme se o Copilot exibiu uma mensagem semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteções de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo a geração, o teste e a validação do código e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou a alteração completa e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente após a revisão e confirmou que o PR foi integrado.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
