---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de determinada forma — ou se há informações que o Copilot deve conhecer —, esse contexto precisa estar acessível. Os [arquivos de instruções][instruction-files] são uma das principais formas de fornecer orientações sobre a geração de código.

Neste exercício, você:

- explorará como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções do repositório e arquivos específicos por caminho.
- gerará uma primeira parte de dados para a filtragem — um auxiliar de editoras — com as instruções atuais.
- adicionará um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviará o mesmo prompt novamente e observará o código gerado adotar o novo padrão.
- integrará a atualização das instruções e a parte de filtragem em `main` para que o Copilot possa usá-las no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a tendência das alterações depois de atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes de unidade.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TypeScript Documentation (TSDoc).
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Use arquivos de instruções para fornecer ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções, bibliotecas e tipos de comentários preferidos. As instruções podem valer para todo o repositório ou para tipos de arquivo específicos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter contexto relevante para a maioria das solicitações, como a stack tecnológica, uma visão geral do projeto, práticas recomendadas e outras orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que podem fornecer diretrizes para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para sugestões de conclusão de código ou de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo `AGENTS.md` e `CLAUDE.md`.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações gerais do projeto, como o que está sendo criado, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes de unidade, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira correta de criar arquivos de instruções ou de usar IA. Experimente para descobrir o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto consistente de arquivos de instruções. Neste projeto, há instruções para [atualizações de interface][ui-instructions] e [Astro][astro-instructions], entre outras tarefas.
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece opções diferentes, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para encontrar templates e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções disponíveis no repositório pelo editor ou pela interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**.
3. Abra a pasta `.github/instructions` e examine os arquivos para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo`, que define um padrão relativo à raiz do repositório. Neste caso, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, são abrangidos.
5. Examine as instruções específicas para criar testes de unidade neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao próprio arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Crie uma branch para as alterações

Crie uma branch para trabalhar nas instruções e no auxiliar de dados.

1. Volte ao codespace do exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de continuar, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indica quando selecioná-los.

Para observar o efeito das instruções personalizadas, envie um prompt com o arquivo de instruções atual. Depois, atualize o arquivo e envie o mesmo prompt novamente.

1. Feche as guias do editor abertas em exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Na visualização Chat, selecione **Agent** no menu de agentes para permitir que o Copilot altere arquivos.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e o ID de cada uma. Aplique as alterações ao arquivo.
   ```

5. O Copilot explorará o projeto e aplicará as alterações, que podem incluir o arquivo auxiliar e os testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas convenções vêm de `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário para o arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários TSDoc sem que você peça. Não há problema: o objetivo é observar a melhora de consistência depois de atualizar as instruções.

## Adicione um padrão para todo o repositório

O arquivo `.github/copilot-instructions.md` fornece orientações gerais ao Copilot. Atualize os padrões do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, próxima da linha 27. Ela documenta os padrões de código, mas ainda não inclui documentação no próprio código.
3. Abaixo dos padrões existentes, adicione:

   ```markdown
   - Todas as funções exportadas devem ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e o valor retornado.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> As instruções podem ficar no nível do repositório, em `.github/copilot-instructions.md`, para orientações gerais, ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar adequado para padrões gerais como a regra de comentários TSDoc.

## Envie o mesmo prompt novamente e observe a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto da conversa.
2. Selecione `src/lib/publishers.ts` novamente para que o Copilot se concentre no arquivo correto.
3. Envie o mesmo prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e o ID de cada uma. Aplique as alterações ao arquivo.
   ```

4. Observe que o arquivo proposto agora começa com um bloco semelhante a:

   ```typescript
   /**
    * Auxiliares de acesso a dados de editoras da plataforma de financiamento coletivo da Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - O cliente de banco de dados do Drizzle.
    * @returns Uma promessa que resolve para uma matriz de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo a código real que será usado no próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são controlados pelo Git como os demais arquivos do repositório. Faça commit das alterações e envie a branch.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme que as alterações das instruções e do auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e integre um pull request

Depois de enviar a branch, crie um pull request associado à issue de documentação do backlog. Peça ao Copilot que localize a issue, crie o PR para fechá-la e faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub em seu nome. O template já configura as ferramentas do GitHub no ambiente. Na primeira utilização, o VS Code talvez peça para você entrar no GitHub. Siga as instruções para autorizar. Você aprenderá como essa conexão funciona — pelo Model Context Protocol — em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que localize a issue de atualização das instruções e crie um PR a partir da branch atual, descrevendo também a base do auxiliar de editoras:

   ```plaintext
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um PR a partir da branch atual, indique que ele fecha essa issue e mencione que também adicionamos a base do auxiliar de editoras para o próximo trabalho de filtragem.
   ```

4. Aguarde o Copilot localizar a issue e criar o PR.
5. Quando solicitado a permitir ações do GitHub, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois da criação do PR, peça ao Copilot que faça o merge e volte para `main`:

   ```plaintext
   Faça o merge do PR em main. Em seguida, volte localmente para main e obtenha o código mais recente para atualizar o repositório.
   ```

7. Quando solicitado a permitir ações do GitHub ou comandos do shell, revise o comando e selecione **Allow** se estiver de acordo.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot usa arquivos de instruções no projeto e, pelo Copilot Chat no VS Code:

- explorou como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot.
- gerou a primeira parte de dados para a filtragem, um auxiliar de editoras, com as instruções existentes.
- adicionou um padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviou o mesmo prompt novamente e observou o código proposto adotar o novo padrão.
- integrou as instruções e a base do auxiliar em `main` para que o próximo exercício possa usá-las.

Em seguida, use essas instruções no [modo de agente][next-lesson] para criar um recurso em várias partes do código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
