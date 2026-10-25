---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo modos de alto contraste e claro. Agora, examine as alterações e, se necessário, envie comentários para aprimorar o trabalho.

### Cenário

Os fundamentos de arquitetura de software e desenvolvimento e operações (DevOps) não mudam com a chegada da inteligência artificial (IA) generativa. O código gerado ainda precisa ser revisado e seguir o processo normal da equipe. Revise as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão constante, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usa para o código.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e temporário.
- Uma pessoa precisa aprovar os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive para servidores MCP.

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: a documentação do código. Use a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso de que o Copilot foi bloqueado por um firewall. Isso **é esperado**: por padrão, o acesso do Copilot a recursos externos, inclusive servidores MCP externos, é limitado. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Acesse seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a marca **[WIP]**. Aguarde a conclusão da tarefa; isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, incluindo os comentários TypeScript Documentation (TSDoc) e outras documentações. As alterações exatas podem variar. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário, definidos nos arquivos de instruções personalizados que você examinou no Exercício 2.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Verifique se há uma indicação de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confira a fila de workflows na seção de verificações do PR. As verificações do projeto para o aplicativo Astro devem ser aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não precisa ser unilateral. Você pode mencionar o Copilot em comentários do PR ou em comentários associados a trechos do código. Ele poderá iniciar outra sessão para atender às solicitações. Como os resultados não são determinísticos, não há um texto único de prompt recomendado. Você pode pedir, por exemplo:

- que adicione cabeçalhos de comentário no início de cada arquivo de código explicando sua finalidade.
- que adicione comentários TSDoc a arquivos TypeScript e Astro.
- que crie um `README` descrevendo a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer integrante da equipe. Use uma das sugestões ou peça outra atualização relacionada à documentação.
2. Selecione **View Session** para acompanhar o Copilot. Ele iniciará uma sessão para fazer as alterações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione **Files changed** e revise as mudanças.

Continue iterando até que o resultado esteja adequado. Depois, você poderá marcar o PR como pronto para revisão e fazer o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, examine o PR criado pelo Copilot para mostrar jogos relacionados na página de detalhes de cada jogo.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Quando terminar, volte à guia **Conversation** e role para baixo.
6. Verifique se há uma indicação de que alguns workflows aguardam aprovação.
7. Se necessário, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Confira a fila de workflows na seção de verificações do PR. As verificações do projeto para o aplicativo Astro devem ser aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** teste manualmente o recurso de jogos relacionados no codespace. Abra um terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Assim como antes, você pode pedir atualizações por meio de comentários na guia **Conversation**; o Copilot os verá e iniciará outra sessão. Por exemplo, peça que ajuste a quantidade de jogos relacionados ou inclua cabeçalhos de comentário e comentários TSDoc. Lembre-se de que essa tarefa foi atribuída **antes** da atualização das instruções personalizadas.

## Revise os recursos de acessibilidade

Por fim, revise os recursos implementados pelo agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no painel de agentes no Exercício 4.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a marca **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione **Files changed** e revise o código gerado.
5. Examine a implementação, especialmente:
   - os componentes de interface para alternar entre os modos.
   - o uso do armazenamento local para persistir as preferências.
   - as alterações de Cascading Style Sheets (CSS) ou estilo para os modos de alto contraste e claro.
   - os atributos de Accessible Rich Internet Applications (ARIA) e a navegação por teclado.
   - o código JavaScript ou TypeScript que gerencia a troca de modos.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Verifique se há uma indicação de que alguns workflows aguardam aprovação.
8. Se necessário, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confira a fila de workflows na seção de verificações do PR. As verificações do projeto para o aplicativo Astro devem ser aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** teste manualmente os recursos de acessibilidade no codespace. Abra um terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme que funcionam e permanecem selecionados após recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar a implementação. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para pedir alterações, como nos outros PRs.

## Exercício opcional — continue delegando

O agente de nuvem é útil quando você pode delegar tarefas reais do backlog e se concentrar em outras atividades. Para praticar, registre algumas issues no repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizados do repositório** para entender como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para implementar alternâncias dos modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar uma sessão de acessibilidade em andamento.
- **Revisou e aprimorou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e a opção de delegar ao agente de nuvem com `/delegate`.

No seu próprio repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

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
