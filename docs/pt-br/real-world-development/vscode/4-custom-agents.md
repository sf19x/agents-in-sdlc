---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial (IA) especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de codificação e conhecimento de domínio que o ajudam a executar determinados tipos de trabalho com mais eficiência. As equipes podem codificar seus conhecimentos em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as Diretrizes de Acessibilidade para Conteúdo Web (WCAG), um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Há uma sobreposição lógica entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e ensinam uma IA a executar operações. A distinção mais simples é: um **agente personalizado** é o executor, e as **skills** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são projetados para coordenar skills — e até outros agentes — durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com diretrizes de acessibilidade. Como parte do trabalho, ele poderia chamar skills, como uma skill de fluxo de trabalho de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única maneira certa de criar um agente personalizado. Como em qualquer trabalho com IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos, independentemente das capacidades ou preferências visuais. Comentários recentes indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que os usuários possam ativar e desativar.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

Como a acessibilidade é fundamental, você usará um agente personalizado para gerar a funcionalidade.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Revise seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace, abra um terminal e crie uma branch a partir de `main` para o trabalho de acessibilidade. Assim, o PR de filtragem do exercício anterior permanecerá separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão às instruções e ferramentas dele.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; cada etapa indicará o que é relevante.

1. Na visualização Chat, confirme que **Agent** está selecionado no menu suspenso de agentes.

   ![Captura de tela do seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu suspenso de agentes na parte inferior da visualização. Por padrão, ele mostra o agente ativo **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. Observe o indicador **Files changed**, que é atualizado à medida que o agente modifica arquivos.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot fará alterações reais no repositório, incluindo componentes Astro, CSS e testes relacionados, conforme necessário.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados para tarefas e domínios específicos. Com agentes personalizados, você pode codificar os conhecimentos e padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que elas aparecem e adicionando uma alternância para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: teste a funcionalidade com o servidor Playwright MCP][previous-lesson] | [Próxima lição: monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
