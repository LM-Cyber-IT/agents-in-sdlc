---
title: "Exercício 4 - Acompanhar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar recursos. Embora ele trabalhe de forma autônoma e assíncrona, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver tarefas de agentes com pull requests (PRs) abertos, examinar as operações executadas e orientar uma sessão em andamento.

Neste exercício, você:

- explorará a página de agentes para acompanhar tarefas do agente de nuvem.
- orientará uma sessão em andamento para pedir uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe decidiu que seria oportuno adicionar também um modo claro. Como o trabalho já envolve atualizar o estilo do site e adicionar opções de alternância, inclua o modo claro na tarefa em andamento.

## Revise as tarefas do agente de nuvem

Confira o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine as tarefas nos painéis principal e esquerdo. Você deverá encontrar tarefas como:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e examine as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - preparar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas dependem do estado do trabalho do Copilot e da abordagem adotada.

4. Examine também o painel de pull request à direita para acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você já viu as tarefas ativas, peça ao Copilot que inclua a opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado da tarefa.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Inclua um controle para que as pessoas possam selecionar o modo de exibição desejado.
   ```

   ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo de orientação destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar a solicitação e incluí-la no trabalho.

## Deixe o Copilot trabalhar

O Copilot incorporará a nova solicitação depois de concluir a etapa em andamento quando você enviou a mensagem.

Esse processo pode levar alguns minutos. Aproveite para refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, seu painel central para acompanhar e orientar tarefas do agente de nuvem. Você acompanhou tarefas ativas e concluídas, examinou o trabalho em andamento e redirecionou uma sessão para ampliar o escopo.

Você:

- explorou a página de agentes e o painel central para acompanhar tarefas do agente de nuvem.
- orientou uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui o trabalho de acessibilidade, [revise e itere sobre os pull requests criados pelo Copilot][next-lesson].

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
