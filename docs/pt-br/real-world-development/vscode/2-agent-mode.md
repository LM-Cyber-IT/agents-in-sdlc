---
title: "Exercício 2 - Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Mesmo uma pequena atualização de um aplicativo pode exigir alterações em vários arquivos e a execução de testes. Em geral, é preciso localizar todos os arquivos relevantes, fazer as mudanças, executar testes, depurar e descobrir se algum arquivo ficou de fora. O processo pode se estender por várias iterações.

O modo de agente do Copilot ajuda a lidar com esse trabalho. Ele explora a estrutura do projeto, faz alterações coordenadas, executa tarefas como testes e pode corrigir problemas encontrados. Use o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona no GitHub, como uma pessoa integrante da equipe, por meio de issues e pull requests.

Neste exercício, você aprenderá a:

- usar o modo de agente do Copilot para explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- implementar recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, as pessoas precisam filtrar por categoria e editora. Você já criou um auxiliar de editoras no exercício anterior. Agora, conclua com o modo de agente do Copilot o trabalho restante na camada de dados, na interface e nos testes.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site para entender suas funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir os detalhes de cada um. O site é um único aplicativo Astro, que gera páginas HyperText Markup Language (HTML) estáticas durante o build. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle, um mapeador objeto-relacional (ORM); não há uma interface de programação de aplicações (API) de back-end separada nem framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um `<script>` Astro pequeno e específico, usando APIs padrão do modelo de objeto de documento (DOM).

### Inicie o site

O projeto inclui a tarefa `npm run dev` para iniciar o servidor de desenvolvimento Astro no Codespace.

1. Volte ao Codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um aviso com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se deseja abrir `http://localhost:4321` no navegador, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o servidor Web em execução no codespace.

### Explore o site

Com o site em execução, explore as funcionalidades disponíveis:

- **Home Page**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Game Details Page**: ao selecionar um jogo, você verá uma página com mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de continuar, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indica quando selecioná-los.

O site inicial já funciona, mas você quer adicionar recursos. O template criou um backlog de issues automaticamente; peça ao Copilot que o apresente.

1. Na lista de agentes da visualização Chat, selecione **Agent**. Esse agente planeja e implementa alterações em vários arquivos, executa comandos do terminal e usa ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores do workshop não estão recomendando um modelo específico. O Claude Sonnet 4.5 foi usado na criação do workshop para tornar as sugestões mais consistentes. Como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

3. Envie ao Copilot este prompt para consultar o backlog:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los de acordo com o que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista gerada.

O Copilot também priorizará as tarefas de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, examine o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Examine as orientações, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de tipos de arquivo, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar partes menores de conjuntos de instruções e fornecer exemplos ao Copilot.

## Implemente a filtragem

Para concluir a filtragem, será necessário atualizar pelo menos três partes do aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento da filtragem.
- atualizar a página de lista de jogos para incluir a interface de filtragem.

Os testes também precisam ser executados e aprovados antes do merge. Peça ao modo de agente do Copilot que realize essas tarefas.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com a issue relacionada no backlog:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria de acordo com os requisitos da issue relacionada no backlog do GitHub. O auxiliar de editoras criado no exercício anterior já existe; preserve e aprimore esse trabalho conforme necessário enquanto conclui as alterações restantes na camada de dados, na interface e nos testes. Confirme que todos os testes foram aprovados antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto, localizar os arquivos da funcionalidade, da camada de dados, da interface e dos testes e, em seguida, alterá-los e executar os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> A exploração, as alterações e os testes podem levar alguns minutos. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto ele ainda estiver trabalhando.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.
8. Alguns testes podem falhar durante o processo. O Copilot alternará entre gerar código e executar testes até concluir o trabalho sem detectar erros.
9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador com o site em execução e experimente a funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Você explorou como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu a:

- explorar o projeto, localizar arquivos relevantes e coordenar alterações.
- implementar recursos na interface e na camada de dados com o modo de agente.
- revisar as alterações e os testes gerados antes do merge.

Em seguida, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra — implemente paginação

À medida que a lista de jogos crescer, será necessário adicionar paginação. Use o que aprendeu neste exercício para pedir ao Copilot que implemente esse recurso. Considere como a paginação deve funcionar, se as pessoas poderão selecionar o tamanho da página ou se ele será fixo. Forneça ao Copilot as orientações necessárias e itere com prompts e contexto adicionais, se necessário.

## Recursos

- [Modos ask, edit e agent do Copilot: para que servem e quando usá-los][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
