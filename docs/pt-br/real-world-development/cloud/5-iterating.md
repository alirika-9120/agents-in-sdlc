---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você atribuiu várias tarefas ao GitHub Copilot para melhorar a experiência do usuário e adicionar recursos. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, pedir ao Copilot que aprimore o trabalho.

### Cenário

Como já destacamos, os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions e cria um ambiente separado e efêmero para trabalhar.
- Uma pessoa precisa aprovar a execução de cada workflow do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o que adiciona documentação ao código. Usaremos a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Nesse caso, aguarde até a conclusão. Isso pode levar alguns minutos. Enquanto isso, faça uma pausa ou reflita sobre o que aprendeu.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os comentários TSDoc recém-criados e outras documentações. As alterações exatas variam.

   Ao revisar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários. Eles vêm dos arquivos de instruções personalizados vistos no início do Exercício 2.

6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Pedir alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários do PR — como faria com outra pessoa da equipe — ou em comentários de linha no código. O Copilot verá esses comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não há um texto prescritivo. Você pode pedir, por exemplo, que o Copilot:

- adicione no início de cada arquivo de código um cabeçalho de comentários com uma descrição breve.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as atualizações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e integrá-lo à branch principal.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, vamos voltar ao PR criado pelo Copilot para resolver a issue sobre jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código criado.
5. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar seu Codespace para essa branch e testar manualmente o recurso de jogos relacionados. Abra o terminal do Codespace e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Você pode continuar pedindo alterações ao agente de nuvem, como ajustar o número de jogos relacionados ou adicionar cabeçalhos de comentários e comentários TSDoc. Lembre-se de que essa issue foi atribuída **antes** da atualização das instruções personalizadas. Para pedir ajustes, adicione um comentário à guia **Conversation**; o Copilot verá a solicitação e iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde até que ele conclua o trabalho, o que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Examine a implementação, com atenção especial a:
   - controles da interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências.
   - alterações de CSS ou estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação pelo teclado.
   - código JavaScript/TypeScript que controla a alternância entre modos.
6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar o Codespace para essa branch e testar manualmente os recursos de acessibilidade. Abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências persistem depois de atualizar a página.

Observe como o agente personalizado de acessibilidade orientou a implementação de acordo com as práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e peça as alterações, como nos PRs anteriores.

## Exercício opcional: continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **inspecionou os arquivos de instruções personalizados do repositório** para observar como influenciam o trabalho do agente de nuvem.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** para acessibilidade, adicionando controles para os modos de alto contraste e claro.
- **usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade.
- **revisou e ajustou pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que aprendeu:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte `/delegate` opcional para o agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologia.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
