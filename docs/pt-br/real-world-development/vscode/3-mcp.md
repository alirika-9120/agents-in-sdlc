---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ele funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem no navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é o padrão que permite ao Copilot se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e de sua interação com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Essas ferramentas e recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e os serviços externos. O servidor gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais em pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos disponíveis para o agente.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios do GitHub, incluindo a criação de repositórios, a atualização dos existentes e o gerenciamento de issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright, permitindo que o agente acesse páginas, preencha formulários e selecione botões.

Há muitos outros servidores MCP disponíveis, com acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e a contribuição ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem com o modelo do projeto. Por isso, o Copilot pôde consultar o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não há nada para instalar localmente. O VS Code conecta-se ao servidor por HTTP, e você autentica-se com o GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor MCP do Playwright

Agora você adicionará outro servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, o que permitirá testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o `playwright` é um servidor **local**. O VS Code inicia-o executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário no codespace, pois ele não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, portanto o navegador normalmente já está instalado. Se o Copilot informar que não há um navegador disponível, peça a ele que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e pedirá que você confirme se confia nele.

1. Em `.vscode/mcp.json`, uma ação **Start** aparecerá acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code pedir que você confirme que confia no servidor, revise a configuração e escolha a opção para confiar nele.

Iniciar o servidor manualmente é opcional. Se você pular esta etapa, o VS Code iniciará o servidor e pedirá que você confie nele quando o Copilot precisar de suas ferramentas.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot acesso a um navegador real. Em vez de percorrer o aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma maneira rápida de confirmar o funcionamento do recurso sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente trabalha com elementos estruturados e rotulados (botões, links e itens de lista), como a tecnologia assistiva, e um teste funcional rápido também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará os resultados. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça que ele corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você tiver interrompido o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente de novo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, você pode abrir um pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está usando o GitHub Copilot, deixe que ele crie o PR e associe-o à issue correspondente.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão nova.
2. No menu de agentes, confirme se **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez você precise autenticar-se. Siga as instruções.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Selecione o link exibido pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou-o para testar manualmente o recurso de filtragem no navegador antes de publicá-lo.
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
