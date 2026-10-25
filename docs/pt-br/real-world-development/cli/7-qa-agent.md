---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de garantia de qualidade que combine requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem no navegador. Agora, reúna esses recursos em um agente personalizado com um processo de garantia de qualidade (QA) bem definido.

Nesta lição, você:

- explorará como um agente personalizado usa instruções, skills e ferramentas do Model Context Protocol (MCP).
- criará e examinará um perfil reutilizável de garantia de qualidade.
- selecionará o agente de QA e revisará as conclusões dele em relação à issue de filtragem.

## Cenário

Antes da abertura de um pull request (PR), a Tailspin Toys quer revisar requisitos, qualidade do código, verificações automatizadas, cobertura de testes e comportamento no navegador de forma consistente. Um agente personalizado pode coordenar esse processo de QA e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. Esse perfil descreve a finalidade do agente, suas instruções e as ferramentas disponíveis. Neste workshop, você definirá uma função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até aqui têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O MCP do Playwright oferece ferramentas de navegador. O perfil de QA orienta o Copilot sobre como usar esses recursos para avaliar os requisitos e relatar as conclusões. Ele não os substitui nem exige outra conversa.

## Crie o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot que crie um perfil reutilizável de QA. O perfil definirá as verificações e os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar os recursos em relação às issues e aos requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o MCP do Playwright para verificar o comportamento e adicionar testes quando houver lacunas de cobertura.

   Peça que ele informe cada requisito como aprovado, reprovado ou bloqueado, com evidências de apoio. Ele deve pedir autorização antes de alterar o código de implementação e não pode fazer commits nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot incluiu o fluxo de QA e os limites de atuação desejados. Isso evita que um agente incompleto ou amplo demais altere o recurso quando você só quer verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. O campo `description` é obrigatório; `name` é opcional, mas dá ao agente um nome claro na interface.
3. Leia as instruções do perfil e confirme que o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme que o QA fornece evidências de apoio, pergunta antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão que o revise antes de continuar.

## Execute o QA em relação à issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Depois de criar o perfil, retome a mesma conversa de filtragem e selecione QA para que o agente possa usar a issue e as decisões de planejamento já incluídas no contexto.

1. Ative o agente com este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como o agente acabou de ser criado, talvez ele ainda não apareça na lista de agentes disponíveis. Use o comando acima para ativá-lo.

2. Envie este prompt para pedir ao agente de QA que revise o recurso:

   ```plaintext
   Revise o recurso de filtragem de acordo com a issue e as decisões do nosso plano. Ele está pronto para um PR?
   ```

3. Aguarde o agente de QA concluir o trabalho.
4. Leia o relatório apresentado.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o resultado. Nesta lição, você:

- explorou como um agente personalizado usa instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as conclusões dele em relação à issue de filtragem.

Agora, a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, [reúna-os em um PR do recurso e use o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
