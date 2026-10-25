---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o workflow e orientar a publicação, mas você continua responsável por revisar as mudanças e confirmar que o site está no ar.

Nesta lição, você:

- pedirá ao Copilot para configurar uma publicação no GitHub Pages.
- revisará o workflow e o pull request antes de fazer o merge.
- acompanhará a publicação e verificará o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages disponibiliza um endereço público (URL) sem adicionar um servidor ou uma dependência de tempo de execução.

## Peça ao Copilot para publicar o site

Peça ao Copilot que configure a publicação e informe a URL somente depois de confirmar que o site está no ar.

1. Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

   ```plaintext
   Habilite o GitHub Pages para este repositório. Crie um workflow do GitHub Actions que faça o build do site, se necessário, e o publique no GitHub Pages. Se ainda não houver um README, adicione um com uma descrição do aplicativo e instruções para executá-lo localmente. Crie um pull request com essas alterações e faça o merge depois que as verificações obrigatórias forem aprovadas. Acompanhe a publicação até que ela seja concluída com sucesso e, então, informe a URL final do site. Se você não tiver permissão para habilitar o Pages ou fazer o merge, informe exatamente o que preciso aprovar ou fazer; não declare sucesso antes de o site estar no ar.
   ```

![Diagrama de três etapas: um pull request mesclado do space-quiz; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e faça merge de um pull request e altere as configurações do Pages do repositório. Revise o diff, as permissões de publicação e todas as solicitações de aprovação do GitHub antes de continuar.

## Revise o workflow e verifique a publicação

Revise as mudanças antes de fazer o merge e confirme que a publicação foi concluída com sucesso.

1. Confirme que o `README` será adicionado somente se o repositório ainda não tiver um.
2. Revise as mudanças do pull request, incluindo as etapas de build e publicação do workflow.
3. Faça o merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de publicação do GitHub Actions até que ela seja concluída com sucesso.
5. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou fazer merge do pull request, siga as instruções específicas fornecidas por ele. Não considere a tarefa concluída até que a publicação seja bem-sucedida e o site esteja carregando.

## Resumo e próximos passos

Você concluiu o fluxo de publicação e confirmou que o site está no ar. Você:

- pediu ao Copilot para configurar a publicação no GitHub Pages.
- revisou o workflow e o pull request antes de fazer o merge.
- acompanhou a publicação e verificou o site publicado.

Em seguida, [revise o fluxo e veja os próximos passos][next-lesson].

[next-lesson]: ../13-review/
