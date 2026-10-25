---
title: "Lição 6 - Validar a funcionalidade com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e explore o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já vimos, programar envolve mais do que escrever código. Também é preciso trabalhar com dados, serviços externos e automações adicionais disponíveis para o Copilot. É aí que entram os servidores do Model Context Protocol (MCP): eles ampliam as ferramentas e os serviços disponíveis além dos que já estão integrados ao CLI.

Nesta lição, você:

- entenderá o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionará o servidor MCP do Playwright, se ele ainda não estiver disponível.
- pedirá ao agente que controle um navegador para explorar o recurso de filtragem.

## Cenário

Testes de unidade e de ponta a ponta são importantes, mas validar alterações na interface também exige interagir com ela. O Copilot pode usar o site como uma pessoa usuária faria e automatizar verificações para aumentar a confiança de que as mudanças funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e os serviços externos. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server][github-mcp]**: oferece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece recursos de automação de navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta e a contribuição para esse ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, pule estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP:

   ```plaintext
   /mcp add
   ```

3. No campo do nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o comando a seguir na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot que explore o recurso com o Playwright

Você já confirmou manualmente que a funcionalidade está correta. Agora, use o servidor MCP do Playwright disponível no Copilot para fazer essa verificação.

1. Envie o prompt a seguir para pedir ao Copilot que valide o recurso usando o servidor MCP do Playwright:

   ```plaintext
   Inicie o aplicativo e use o MCP do Playwright para verificar a filtragem de acordo com a issue e nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Quando terminar, interrompa o servidor que você iniciou.
   ```

> [!NOTE]
> Em geral, não é necessário indicar um servidor MCP específico, pois o Copilot costuma identificá-lo pelo contexto. Ainda assim, você pode apontar a ferramenta que considera importante para obter resultados mais consistentes.

2. Observe o Copilot listar as etapas que está executando no navegador para verificar o recurso.
3. Leia o relatório e confirme que tudo funciona como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor MCP do Playwright para explorar o recurso em um navegador real pelo Copilot CLI. Você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador para explorar o recurso de filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que reúna a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando disso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
