---
slug: pt-br/real-world-development/app
title: "Aplicativo GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** é um aplicativo para desktop baseado no Copilot CLI que reúne o desenvolvimento orientado por agentes em um espaço de trabalho dedicado. Ele adiciona sessões paralelas de agentes, modos de sessão alternáveis, canvases compartilhados e gerenciamento nativo de issues e pull requests do GitHub — incluindo o **Agent Merge**, que conduz um pull request por rebases, feedback de revisão, correções de integração contínua (CI) e merge.

O workshop segue um fluxo contínuo da Tailspin Toys:

1. Prepare o projeto, instale o aplicativo, conecte o repositório e explore o espaço de trabalho e o backlog predefinido.
2. Faça uma alteração específica nas avaliações por estrelas, revise-a no navegador e integre manualmente o primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo **Plan**, implemente-a no modo **Autopilot** e depois revise-a no modo **Interactive**.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill `quality-checks` existente e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise toda a alteração de filtragem e use o Agent Merge no segundo PR.
9. Verifique a disponibilidade de canvas; depois, use o Database Explorer e crie e teste um canvas de triagem vinculado ao repositório. Se os recursos necessários não estiverem disponíveis, registre o bloqueio e use a alternativa somente leitura ou continue para o resumo.

Para manter o foco do workshop, você criará dois PRs: um para avaliações por estrelas e outro para filtragem, com as atualizações de instruções e da skill, o perfil de QA e os testes. Comece cada um a partir de `main` atualizado. O fluxo de filtragem e qualidade compartilha uma sessão, uma worktree e uma branch para que você aproveite seu trabalho ao explorar cada ferramenta. O exercício final de canvas permanece na própria sessão para que você se concentre em criar e testar a superfície compartilhada sem repetir o fluxo de PR.

## Lições

| Lição | Tópico | O que você fará |
| ------ | ----- | --------------- |
| [0. Pré-requisitos][ex0] | Configuração | Instalar o Node.js e criar sua cópia do projeto Tailspin Toys |
| [1. Instalar o aplicativo Copilot][ex1] | Configuração | Instalar o aplicativo, conectar o projeto e conhecer o espaço de trabalho |
| [2. Adicionar avaliações por estrelas: uma melhoria rápida][ex2] | Primeira alteração | Exibir as avaliações existentes e a alternativa para valores nulos; depois, integrar o PR 1 |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planejar o recurso a partir da issue, implementá-lo com Autopilot e revisá-lo no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explorar e atualizar as instruções e aplicá-las à filtragem |
| [5. Personalizar e usar a skill `quality-checks`][ex5] | Verificações repetíveis | Explorar a skill existente, alterar o formato do relatório e executá-la |
| [6. Validar a funcionalidade com o MCP do Playwright][ex6] | Observação no navegador | Configurar o MCP em Customize e inspecionar o comportamento da filtragem |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Criar e selecionar um perfil especializado e reunir as evidências finais de verificação |
| [8. Criar e integrar o PR do recurso][ex8] | Revisão e merge | Revisar a filtragem, as instruções, a skill, o perfil de QA e os testes; depois, usar o Agent Merge no segundo PR |
| [9. Explorar e criar canvases][ex9] | Colaboração | Usar o Database Explorer e criar e testar um canvas de triagem vinculado ao repositório |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revisar o fluxo, os artefatos e outros recursos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Um computador com **macOS, Linux ou Windows**
- [ ] O [Git instalado][install-git] no computador

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop — você pode concluir todos os percursos com ele.

> [!NOTE]
> Como o aplicativo Copilot é executado no seu computador, e não em um codespace, a [lição de pré-requisitos][ex0] orienta você na instalação do Node.js e na criação da sua cópia do projeto antes de instalar o aplicativo.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política do **aplicativo GitHub Copilot**. Essa política é diferente da política do Copilot CLI.

## Começar

**[Comece pelos pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
