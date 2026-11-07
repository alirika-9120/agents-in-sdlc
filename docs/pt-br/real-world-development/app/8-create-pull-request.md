---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisar tudo em conjunto e abrir um pull request. Você mesmo integrou o pull request (PR) de avaliações por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações de instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma sessão e branch, e todo esse trabalho será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar o PR completo do recurso e as evidências de verificação.
- autorizar o Agent Merge somente após a revisão e confirmar que o PR foi integrado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle da pessoa desenvolvedora.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver bloqueando o merge — por exemplo, verificações de integração contínua (CI) com falha, comentários de revisão ou a necessidade de rebase — e faz o merge assim que o GitHub permite. Ele funciona em segundo plano, continua após reinicializações do aplicativo e é desativado automaticamente quando o pull request é integrado.

Até aqui, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade, mas sua capacidade de editar código e fazer o merge ainda exige sua autorização explícita. Revise as ações permitidas e o trabalho antes de conceder permissão para o merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge será iniciado. Ele vai:

- criar o pull request com título e descrição.
- se você iniciou a sessão a partir de uma issue, incluir uma referência à issue relacionada no corpo da descrição.
- fazer rebase ou lidar com possíveis conflitos de merge na branch de destino.
- monitorar o processo de CI para garantir que todas as verificações sejam aprovadas.
- monitorar o PR em busca de comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer atualizações para resolvê-los.
- opcionalmente, integrar o PR automaticamente quando todas as verificações forem concluídas com êxito.

Vamos permitir que o Agent Merge também integre o PR depois que tudo for aprovado.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para o merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente para integrar o PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva todos os bloqueios relatados. Antes de continuar, confirme que o status do PR é **Merged**; a permissão de merge habilitada ou um agente em execução não confirma que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem a falta de permissões. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo a geração, o teste e a validação do código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou o PR completo do recurso e as evidências de verificação.
- autorizou o Agent Merge somente após a revisão e confirmou que o PR foi integrado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma maneira mais rica de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
