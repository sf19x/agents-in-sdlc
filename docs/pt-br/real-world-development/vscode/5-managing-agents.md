---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo. Você pode revisar diffs, aceitar ou rejeitar alterações individuais e orientar a conversa com novos prompts sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um modo claro junto com o alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho continua, a equipe de design solicita também uma alternância para o modo claro, destinada a usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas na conversa. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Abra também o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente pausará e exibirá **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde até que o agente conclua a implementação do modo de alto contraste. Deve aparecer uma mensagem final que resume as alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, amplie a mesma conversa para adicionar uma alternância de modo claro. Como você continuará na mesma sessão, o agente manterá o contexto dos componentes, dos estilos e do padrão de armazenamento local usados anteriormente.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações já feitas e propondo adições para o modo claro.
3. Revise cada nova edição quando aparecer, usando **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das alternâncias de um jeito que você não quer —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique as folhas de estilo em cascata (CSS) — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira alternância; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas alternâncias").

## Revise o conjunto de alterações

Antes de confirmar o trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e nos testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se a implementação segue os padrões de acessibilidade do agente personalizado, incluindo atributos Accessible Rich Internet Applications (ARIA), navegação por teclado, HyperText Markup Language (HTML) semântica e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e confirme as alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como monitorar o trabalho do agente em tempo real.
- como aceitar ou rejeitar edições individuais.
- como orientar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: agentes personalizados][previous-lesson] | [Próxima lição: iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
