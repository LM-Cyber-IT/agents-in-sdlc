---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações deixam de enfrentar dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou recursos no backlog há muito tempo por falta de tempo para implementá-los. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, cria um pull request (PR) de rascunho para que uma pessoa desenvolvedora revise o trabalho. Assim, tarefas repetitivas podem ser delegadas, acelerando o desenvolvimento e liberando a equipe para se concentrar em questões mais amplas.

Neste exercício, você explorará como:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer reduzir a dívida técnica. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado: ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TypeScript Documentation (TSDoc) a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar outros títulos da mesma categoria para que as pessoas possam continuar explorando o catálogo. A equipe não precisa de uma implementação finalizada; quer uma versão que possa usar para testar a experiência. Embora essa tarefa esteja bloqueando o trabalho, há issues de prioridade mais alta no momento.

Essas tarefas podem ser adiadas facilmente, por isso são boas candidatas à delegação ao agente de nuvem do Copilot. Ele trabalha de forma assíncrona enquanto você se dedica a outras tarefas; depois, você pode revisar o resultado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Uma opção é [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um PR de rascunho para acompanhar o progresso, prepara um ambiente e começa a trabalhar. Você pode examinar a sessão durante ou depois da execução. Quando a solução estiver pronta para revisão, o Copilot mencionará você no PR.

## A importância de instruções com escopo claro

O GitHub Copilot não elimina a necessidade de pensar. Mesmo tarefas aparentemente simples podem ter complexidades que só aparecem quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas atribuídas ao agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu um exercício sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, aproveite para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções existentes. Assim, você poderá observar seus efeitos mais tarde.

Abra os seguintes arquivos na interface Web do GitHub ou em um codespace:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, em especial as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Quando atribuir a issue `Code lacks documentation` ao agente de nuvem, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário no pull request resultante. Essas orientações vêm dos arquivos de instruções. Você voltará a examiná-las ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e scripts de configuração para preparar as dependências. Isso também vale para tarefas atribuídas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de execução. Você pode personalizá-lo com um [workflow de configuração][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. O template já configura esse workflow para facilitar o laboratório. Ele instala o Node.js e as dependências, os navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório durante a configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se o repositório não for clonado nas etapas de configuração, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar que o Copilot inicie o trabalho.

## Adicione documentação ao código

Embora a importância da documentação seja amplamente reconhecida, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica pode reduzir a produtividade e dificultar a manutenção do código ou a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação, e esta é uma boa tarefa para atribuir ao agente de nuvem. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Nossa organização exige comentários TSDoc em funções e métodos quando forem úteis. Infelizmente, atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários quando eles esclarecerem o comportamento.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o novo PR, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente com o workflow descrito anteriormente, analisando o projeto e definindo uma abordagem para a tarefa.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do PR e localize uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Examine a sessão em andamento para acompanhar como o Copilot explora o código, entende o estado do projeto, planeja e cria código.

O processo pode levar vários minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Aproveite esse recurso atribuindo outra tarefa e, em seguida, trabalhe em um recurso do aplicativo.

## Adicione uma seção de jogos relacionados

Uma vantagem do agente de nuvem é poder dividir o trabalho: enquanto ele cuida de uma tarefa, você pode se dedicar a outra. A seção de jogos relacionados pode não levar muito tempo, mas delegá-la libera tempo para outras atividades. Atribua essa tarefa ao agente de nuvem.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos exibindo títulos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer alguns testes de aceitação. Estes são os requisitos:

   - Adicione em `src/lib/` um auxiliar de acesso a dados que retorne outros jogos da mesma categoria de um determinado jogo, sem incluir esse jogo.
   - Exiba na página de detalhes uma seção **Related games** que use o auxiliar.
   - Trate o caso em que um jogo não tenha títulos relacionados.
   - Crie testes de unidade para o novo auxiliar.
   - Antes de criar o PR, confirme que todos os testes foram aprovados.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente. Revise o resultado quando ele terminar. Agora, passe à criação e ao uso de agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona e delegar tarefas como reduzir dívida técnica, criar recursos ou migrar código entre frameworks.

Você explorou como:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e instruções personalizadas, apresentadas em exercícios anteriores.

## Recursos

- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
