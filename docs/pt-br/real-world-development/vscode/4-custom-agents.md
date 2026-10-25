---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de inteligência artificial (IA) especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Defina-os com arquivos Markdown na pasta `.github/agents` do repositório para fornecer instruções focadas, práticas recomendadas, padrões de código e conhecimento específico. As equipes podem transformar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as diretrizes [de acessibilidade para conteúdo da Web (Web Content Accessibility Guidelines, WCAG)][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido por um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam a IA sobre como realizar tarefas. Uma forma simples de diferenciá-los é considerar o agente personalizado como quem executa o trabalho e as skills como ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem coordenar skills e outros agentes. Neste laboratório, o agente de acessibilidade revisa e atualiza o site de acordo com diretrizes de acessibilidade. Para isso, poderia usar uma skill de fluxo de pull request ou uma skill que executa e gerencia testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Teste e itere para descobrir o que funciona no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre o texto e o plano de fundo. Para resolver o problema, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para criar essa funcionalidade rapidamente.

Neste exercício, você:

- revisará um agente personalizado de acessibilidade existente.
- usará esse agente no Copilot Chat para implementar um modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Examine seu conteúdo para entender como ele orientará o Copilot.

1. Volte ao codespace e abra um terminal.
2. Crie uma branch a partir de `main` para o trabalho de acessibilidade, mantendo separado o PR de filtragem do Exercício 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. Abra `.github/agents/accessibility.md`.
4. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

5. Examine as seções seguintes, que descrevem:
   - as responsabilidades principais ao criar código para um site acessível.
   - as práticas recomendadas de acessibilidade.
   - exemplos de código HyperText Markup Language (HTML), folhas de estilo em cascata (Cascading Style Sheets, CSS) e JavaScript.
   - armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados definidos em `.github/agents` no seletor de agentes na parte inferior da visualização Copilot Chat. Selecione um agente para aplicar suas instruções e ferramentas à conversa.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de continuar, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indica quando selecioná-los.

1. Na lista de agentes da visualização Chat, selecione **Agent** se ainda não estiver selecionado.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior do Chat, que mostra o agente ativo — por padrão, **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Adicione um modo de alto contraste ao site. Inclua um controle que permita ativar e desativar esse modo e salve a preferência em localStorage para que ela continue valendo após recarregar a página.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. A indicação **Files changed** será atualizada para mostrar os arquivos modificados pelo agente.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot alterará arquivos reais do repositório, como componentes Astro, estilos CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados em tarefas e domínios. Com eles, é possível transformar a experiência e os padrões da equipe em agentes reutilizáveis.

Você explorou como:

- os agentes personalizados são definidos.
- usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações durante a execução e adicione um modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Acompanhar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
