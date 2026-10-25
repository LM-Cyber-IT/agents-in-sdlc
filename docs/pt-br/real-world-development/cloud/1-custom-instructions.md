---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de determinada forma — ou se há informações que o Copilot deve conhecer —, esse contexto precisa estar acessível. Os [arquivos de instruções][instruction-files] oferecem orientações para que o Copilot entenda não só *o que* você quer, mas também *como* deseja que o trabalho seja feito.

Neste exercício, você:

- explorará como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções do repositório e arquivos aplicáveis a caminhos específicos.
- adicionará um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos percursos do VS Code e do CLI, aqui você não comparará prompts de antes e depois. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o efeito lado a lado em tempo real. Você verá a influência das instruções mais adiante, ao revisar os pull requests que o agente criar.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes de unidade.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TypeScript Documentation (TSDoc).
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Use arquivos de instruções para fornecer ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Elas ajudam a orientar sugestões de código. Você pode especificar convenções, bibliotecas e tipos de comentários preferidos. As instruções podem valer para todo o repositório ou para tipos de arquivo específicos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter contexto relevante para a maioria das solicitações, como a stack tecnológica, uma visão geral do projeto, práticas recomendadas e outras orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que podem fornecer diretrizes para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes de unidade.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para sugestões de conclusão de código ou de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo `AGENTS.md` e `CLAUDE.md`.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações gerais do projeto, como o que está sendo criado, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes de unidade, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira correta de criar arquivos de instruções ou de usar IA. Experimente para descobrir o que funciona melhor no projeto.

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

Leia os arquivos de instruções disponíveis no repositório pela interface Web do GitHub ou no codespace.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**. Essas instruções se aplicam às interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo`, que define um padrão relativo à raiz do repositório. Neste caso, os arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, são abrangidos.
5. Examine as instruções específicas para criar testes de unidade neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Os caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TypeScript Documentation (TSDoc) e cabeçalhos de comentário nos arquivos.

## Adicione um padrão para todo o repositório

Adicione uma regra ao arquivo principal para que o agente de nuvem use comentários TSDoc e um cabeçalho de comentário nos arquivos ao trabalhar nas issues atribuídas depois.

1. No terminal do codespace, crie uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra novamente `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, próxima da linha 27. Observe que ainda não há uma regra para documentação no código.
4. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Todas as funções exportadas devem ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e o valor retornado.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> As instruções podem ficar no nível do repositório, em `.github/copilot-instructions.md`, para orientações gerais, ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar adequado para padrões gerais como a regra de comentários TSDoc.

## Faça commit, envie e integre a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. No próximo exercício, ele criará uma branch a partir de `main`; portanto, as instruções precisam ser integradas a `main` para que sejam aplicadas.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem poderá usar as instruções nas issues do próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode pular a criação da branch e fazer o commit diretamente em `main`. A branch foi incluída para simular o fluxo de um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa arquivos de instruções. Neste exercício, você:

- explorou como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e dos arquivos aplicáveis a caminhos específicos.
- adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

Você verá esse padrão nos pull requests criados pelo agente de nuvem nos próximos exercícios.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
