---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi desenvolvido para trabalhar com o fluxo existente de uma organização e ajudar a criar as soluções certas da maneira certa. Nesta e nas próximas lições, você seguirá um processo típico de desenvolvimento orientado por agentes: usará uma issue para criar um recurso, verificará se o código está correto e se o recurso funciona como esperado e, por fim, fará o merge dele no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo do recurso. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivo, mas vamos simplificar para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa com o Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o recurso usando o modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar esse trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a codificação. Você usará o modo Plan para resolver essas questões e, em seguida, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Começar por uma issue registrada que descreve o trabalho.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com êxito, fazer o merge do código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria dos fluxos é uma variação do processo acima.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua confirmação antes de continuar.
- **Plan**: o agente cria um plano primeiro e fica impedido de editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme se o PR das avaliações por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que agora o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou um pull request (PR) do GitHub.

## Planejar o recurso de filtragem

O planejamento permite definir a abordagem antes de delegar a implementação ao Copilot. Para tarefas complexas, vale a pena reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano explorando o projeto e definindo a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos pedir ao Copilot que trabalhe usando o Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar o trabalho concluído.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme se o indicador abaixo do prompt mostra **Autopilot**.
3. Observe o Copilot seguir o plano, gerar código e executar testes de forma iterativa.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto, revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes do merge. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e examine a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pelo encaminhamento de porta do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e as combinações acordadas no plano.
4. Confirme se o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado por ele.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa com o Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o novo recurso usando o modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos explorar melhor como o código é gerado e garantir que siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
