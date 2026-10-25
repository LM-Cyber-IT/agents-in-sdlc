---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revise-a no navegador encaminhado e integre-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem avaliações por estrelas, mas os cartões da página inicial ainda não as exibem. Peça ao agente que mostre as avaliações, revise a alteração e faça o merge do seu primeiro pull request.

Nesta lição, você:

- iniciará uma conversa focada com o Copilot em uma branch de recurso.
- pedirá ao agente que faça uma pequena alteração no projeto.
- revisará a alteração com `/diff`.
- executará o aplicativo e confirmará a mudança no navegador encaminhado.
- abrirá e fará o merge do primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Os cartões da página inicial, porém, exibem apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente que mostre a avaliação existente em cada cartão — uma alteração pequena e independente, ideal para sua primeira sessão.

## Como é uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Ao contrário do aplicativo Copilot, uma conversa normal no CLI usa o repositório e a branch do Git que estão atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas que ele usa para explorar e editar arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e peça a alteração

Inicie uma nova conversa para começar a implementar o recurso.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, inicie-o com este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Envie o prompt a seguir para pedir a alteração:

   ```plaintext
   Mostre o `starRating` de cada jogo, em uma escala de 5, nos cartões da página de listagem. Se a avaliação for `null`, mostre exatamente "No rating yet". Mantenha o layout dos cartões, adicione testes e execute as verificações relevantes.
   ```

O Copilot explorará o projeto, localizará os arquivos que exibem os detalhes dos jogos e criará o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Inspecione as mudanças diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão do jogo exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como o Copilot, assim como outras ferramentas de IA generativa, é probabilístico, e não determinístico, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica a um exemplo.

## Verifique as alterações

Não basta ler o código e presumir que ele funciona. Peça ao Copilot que inicie o site para você examinar a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração das avaliações por estrelas. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que ele pare o servidor iniciado:

   ```plaintext
   Interrompa o servidor de desenvolvimento que você iniciou.
   ```

## Abra e faça o merge do primeiro pull request

Você criou o recurso. Agora, crie um pull request (PR) para integrar o novo código ao projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas nas avaliações por estrelas com uma mensagem de commit apropriada.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e mostrará a URL do PR.
3. Abra o PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando o localizador uniforme de recursos (URL) exibido pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração com o GitHub Copilot CLI. Você:

- iniciou uma conversa focada com o Copilot em uma branch de recurso.
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo e confirmou a avaliação por estrelas no navegador encaminhado.
- abriu e integrou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
