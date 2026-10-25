---
title: "Exercício 5 - Acompanhar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real no computador, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições à medida que acontecem. Você pode revisar cada diff, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts adicionais, sem sair do editor.

Neste exercício, você:

- acompanhará em tempo real o trabalho do agente de acessibilidade.
- revisará e aceitará as edições propostas nos arquivos.
- orientará o agente durante a sessão para adicionar um modo claro junto ao alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design pediu também uma opção de modo claro para quem considera o tema padrão muito escuro. Em vez de começar outra conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar as chamadas de ferramentas, leituras de arquivos, edições e comandos de terminal.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para desfazê-la e permitir outra tentativa do agente.
4. Você também pode abrir o indicador **Files changed** na parte superior do Chat para ver todos os arquivos modificados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão, por exemplo, para executar um comando de terminal, o agente aguardará e mostrará **Allow**. Aprove apenas os comandos com os quais você concorda.

5. Aguarde o agente concluir o modo de alto contraste. Ele deverá apresentar uma mensagem final resumindo as alterações.

## Oriente o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar uma opção de modo claro. Como você continua na sessão existente, o agente mantém o contexto dos componentes, estilos e padrões de armazenamento local utilizados.

1. No campo de entrada do Copilot Chat, envie este prompt:

   ```plaintext
   Bom trabalho! Agora, adicione uma opção de modo claro junto ao controle de alto contraste. Siga o mesmo padrão: persista a preferência em localStorage e disponibilize o controle na mesma área da interface. As pessoas devem poder ativar o modo de alto contraste e o modo claro de forma independente.
   ```

2. O agente examinará as alterações já feitas e proporá as mudanças para o modo claro.
3. Revise cada nova edição e selecione **Keep** ou **Undo** no diff.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar a interface dos controles de um modo que você não quer —, pare-o com o botão **Stop** ao lado do campo de entrada e envie uma orientação mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Envie prompts adicionais para pedir ajustes ("simplifique o Cascading Style Sheets (CSS) e use variáveis CSS em vez de duplicar seletores"), corrigir a abordagem ("não adicione um terceiro controle; inclua o modo claro no painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois controles").

## Revise o conjunto de alterações

Antes de fazer commit, examine os arquivos alterados pelo agente.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos. Você deverá encontrar alterações em componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme que a implementação segue os padrões de acessibilidade do agente personalizado, como atributos Accessible Rich Internet Applications (ARIA), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na próxima lição.

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts adicionais para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão com prompts adicionais.

Na [próxima lição][next-lesson], você publicará as alterações de acessibilidade em um pull request e revisará o trabalho do agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
