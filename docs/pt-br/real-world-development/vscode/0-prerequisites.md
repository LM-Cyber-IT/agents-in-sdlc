---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo. Você criará sua própria cópia do repositório Tailspin Toys, iniciará um [codespace][codespaces] e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configure o repositório do laboratório

Crie uma instância a partir do [repositório de template][template-repository]. Ela conterá os arquivos necessários para o laboratório e será usada durante os exercícios.

1. Em uma nova janela do navegador, acesse o [template da Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e depois **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as orientações das pessoas mentoras. Caso contrário, crie o repositório em uma organização onde tenha acesso ao GitHub Copilot.

   ![Configurações do repositório criado a partir do template](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (`organization-or-user-name/repository-name`), pois você precisará dele mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar o repositório a partir do template, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop; não precisa criar nenhuma manualmente.

## Crie um codespace

Use um codespace para concluir os exercícios.

[O GitHub Codespaces][codespaces] oferece um ambiente de desenvolvimento na nuvem para escrever, executar e depurar código diretamente no navegador. Ele fornece um ambiente de desenvolvimento integrado (IDE) completo, com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório criado.
2. Selecione o botão verde **Code**.

   ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, o botão **+** para criar um codespace.

   ![Criação de um codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace pode levar alguns minutos, mas ainda é mais rápida do que instalar todos os serviços manualmente. Enquanto isso, você pode explorar outros recursos do GitHub Copilot que usará em seguida.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [dev container local][dev-containers]. Ambos fornecem os pré-requisitos necessários. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo dev container usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Use o GitHub Copilot Chat e o modo de agente

Para usar o modo de agente do GitHub Copilot Chat, a extensão GitHub Copilot Chat precisa estar instalada no IDE. Ela já deve estar disponível se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão não estiver instalada, [instale-a pelo Visual Studio Code Marketplace][copilot-chat-extension]. Você também pode abrir a visualização **Extensions** no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois da instalação, talvez seja necessário autenticar sua conta do GitHub para habilitar a extensão.

1. Volte ao codespace.
2. Se o Copilot Chat não estiver à direita do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem, como `Hello world`, na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deve ativar o Copilot Chat.
4. Se você ainda não estiver autenticado, siga as instruções para entrar na sua conta do GitHub.

   ![Exemplo de prompt de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Depois da autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou a criação do codespace que usará para programar.

## Próxima etapa

Agora, comece a trabalhar com o Copilot. Continue para [Exercício 1: Instruções personalizadas][next-lesson] e apresente ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Primeiros passos com o Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
