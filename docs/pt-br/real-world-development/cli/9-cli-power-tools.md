---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como outras ferramentas de linha de comando, o GitHub Copilot CLI inclui vários comandos de barra. Eles oferecem recursos avançados, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Agora, explore outros comandos úteis.

Nesta lição, você:

- usará `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usará `/model` para explorar os modelos disponíveis.
- aprenderá como `/share` exporta ou compartilha uma sessão.
- explorará comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo principal do CLI. Agora, examine alguns recursos adicionais: gerenciamento de contexto, troca de modelos, compartilhamento de sessões e delegação opcional ao [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Em tarefas maiores ou mais complexas, você pode se aproximar do limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, e você também pode inspecionar ou compactar o contexto com comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. A diferença pode ser pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela se preenche. Use `/compact` para escolher o momento. Use `/clear` ou `/new` para começar uma conversa nova ao mudar para uma tarefa sem relação com a atual.

## Escolha um modelo

Os modelos têm pontos fortes diferentes, e cada pessoa desenvolvedora pode ter preferências próprias. O Copilot CLI permite listar e selecionar um modelo.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Compartilhar conhecimentos com a equipe ajuda todas as pessoas a aprimorar o uso de ferramentas de inteligência artificial (IA). O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HyperText Markup Language (HTML), criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e examine as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que sua equipe não pretende compartilhar.

## Opcional: amplie o trabalho ou delegue

O fluxo principal do workshop está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode distribuir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar um worktree Git isolado para outra tarefa.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e talvez abra um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou iniciar trabalho remoto. Comece uma tarefa nova e bem delimitada antes de experimentá-los e revise os resultados pelo fluxo normal. Para saber mais sobre trabalho assíncrono com agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra permitem configurar o Copilot CLI, compartilhar sessões e examinar o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há outros comandos de barra e recursos para conhecer. Para concluir este percurso, [revise o que você aprendeu e os próximos passos][next-lesson].

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
