---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a filtragem com o modo de agente. Antes de abrir um pull request (PR), confirme que ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor do Model Context Protocol (MCP) do Playwright** e peça ao Copilot que controle um navegador real. Depois, publique sua branch e abra o PR.

Neste exercício, você:

- aprenderá o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionará o servidor MCP do Playwright ao espaço de trabalho.
- pedirá ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicará a branch e abrirá um pull request com o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais poderoso quando pode usar ferramentas externas ao editor. O Model Context Protocol (MCP) é um padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento do modo de agente e de suas interações com contexto, modelo de linguagem de grande porte (LLM) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Um servidor MCP funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Ele gerencia a comunicação com interfaces de programação de aplicações (APIs) existentes ou ferramentas locais, como pacotes npm. Cada servidor oferece um conjunto de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com o Playwright para navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição para o ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Confirme que já há um servidor `github` configurado:

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

Essa configuração vem do template do projeto e permitiu que o Copilot acessasse o backlog de issues no exercício anterior. O [servidor GitHub MCP remoto][remote-github-mcp-server] é acessado por HTTP; não é necessário instalá-lo localmente. Na primeira utilização das ferramentas, você precisará se autenticar no GitHub.

## Adicione o servidor MCP do Playwright

Adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

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

Ao contrário do servidor remoto `github`, o servidor `playwright` é local: o VS Code o inicia no computador usando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária em um codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário geralmente está instalado. Se o Copilot informar que está faltando, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: quando o Copilot precisa usar as ferramentas de um servidor pela primeira vez, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code pedir para confirmar que você confia no servidor, revise a configuração e autorize a inicialização.

Iniciar o servidor manualmente é opcional. Se você pular esta etapa, o VS Code o iniciará e pedirá confirmação na primeira utilização das ferramentas pelo Copilot.

## Teste o recurso de filtragem

O servidor MCP do Playwright permite que o Copilot controle um navegador real. O agente pode abrir uma página, navegar, aplicar filtros e relatar os resultados. Isso permite verificar o comportamento sem sair da conversa.

O servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. Assim, o agente interpreta elementos estruturados e identificados, como botões, links e itens de lista, de forma semelhante à tecnologia assistiva. Uma verificação funcional rápida também pode servir como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

```text
Use o servidor MCP do Playwright para abrir um navegador no aplicativo em execução em http://localhost:4321 e verificar o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e anote quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme que a lista mostra somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme que a lista mostra somente jogos dessa editora.
4. Combine os filtros de categoria e editora e confirme que os resultados correspondem aos dois critérios.

Relate o que observar em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e informará o que observou. Compare o relatório com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas de acompanhamento ou peça que o Copilot corrija o código antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor, inicie-o novamente antes de enviar o prompt. Na primeira utilização do servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para abrir um pull request (PR) e permitir a revisão da equipe.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

É possível criar um PR no github.com ou pela interface de linha de comando do GitHub. Como você já está usando o GitHub Copilot, peça que ele encontre a issue relacionada e crie o PR associado.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Confirme que **Agent** está selecionado na lista de agentes para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Encontre no repositório a issue relacionada aos filtros por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar-se.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Abra o link fornecido pelo Copilot para revisar o PR, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente o recurso de filtragem em um navegador.
- publicou a branch e abriu um pull request com o trabalho.

Você usou um servidor MCP para testar um recurso, mas essa é apenas uma das formas de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando disso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
