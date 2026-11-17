---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: instruções personalizadas][previous-lesson] |
|:--|

Quase todas as organizações enfrentam dívida técnica, como problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram no backlog por falta de tempo. O agente na nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) de rascunho para revisão por um desenvolvedor. Isso permite delegar tarefas repetitivas, acelerar o desenvolvimento e liberar tempo para trabalhos mais amplos.

Com o agente na nuvem do Copilot, você explorará:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte da dívida técnica. A documentação deixada pelos prestadores que criaram a primeira versão do site está em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência. Essa tarefa é um bloqueio, mas outras issues têm prioridade maior no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são adequadas para atribuir ao agente na nuvem. Ele trabalha de forma assíncrona, permitindo que o desenvolvedor cuide de outras tarefas e depois volte para revisar o trabalho.

## Conheça o agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a um desenvolvedor. Assim como no trabalho com outra pessoa, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request (PR) de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot mencionará você no PR.

## A importância de instruções com escopo definido

Não há mágica no GitHub Copilot: você ainda precisa pensar. Até operações aparentemente simples têm complexidades que nem sempre são óbvias. Por isso, [defina com cuidado o escopo das tarefas para o agente na nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente na nuvem, leia os arquivos de instruções existentes sem alterá-los para que possa observar seus efeitos depois. O servidor do Model Context Protocol (MCP) do GitHub também estará disponível para tarefas com issues e pull requests.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as convenções de TypeScript e as expectativas para comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Na próxima seção, quando atribuir ao agente a issue `Code lacks documentation`, observe nos pull requests os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções e serão retomadas em uma lição posterior, durante a revisão dos PRs.

## Configure o ambiente de desenvolvimento do agente na nuvem

Criar código normalmente exige um ambiente específico e scripts de configuração para garantir que tudo esteja pronto. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de trabalho de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Isso disponibiliza as ferramentas e dependências necessárias. O fluxo já está configurado para este laboratório: instala o Node.js e as dependências do projeto, os navegadores para testes de ponta a ponta e configura o banco de dados SQLite migrado e preenchido usado pelo único aplicativo Astro.

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros fluxos de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para verificar se ele funciona sem esperar uma execução do Copilot.

## Adicione documentação

Embora a importância da documentação seja conhecida, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica pode reduzir a produtividade e dificultar a manutenção da base de código ou a integração de novos desenvolvedores. O Copilot pode ajudar a criar documentação, por isso esta é uma issue adequada para o agente na nuvem. Você voltará em um exercício futuro para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente ao executar o fluxo de trabalho descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o agente pretende executar.
14. Role a linha do tempo do pull request para baixo. Deverá aparecer uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Explore a sessão em tempo real para acompanhar a solução: o Copilot examina o código e o estado do projeto, pausa para planejar e, em seguida, cria o código.

Isso provavelmente levará alguns minutos. Um dos objetivos do agente na nuvem é executar tarefas de forma assíncrona para que você possa se dedicar a outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa e, em seguida, trabalhando em uma funcionalidade do aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente na nuvem é dividir o trabalho: você pode cuidar de uma tarefa enquanto o agente cuida de outra. Vamos atribuir a ele a criação de uma seção de jogos relacionados para a equipe de design.

1. Volte ao repositório no github.com.
2. Selecione **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, deverá aparecer um conjunto de olhos 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na segunda solicitação. Como o agente na nuvem funciona de maneira semelhante a um engenheiro de software, não é necessário acompanhá-lo continuamente; revise o trabalho quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximas etapas

Nesta lição, você explorou o [agente na nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao Copilot para execução assíncrona e usá-lo para resolver dívida técnica, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha, você pode criar e usar agentes personalizados. O [agente na nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, que foram abordados anteriormente.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente na nuvem][cloud-agent-best-practices]

| [Próxima lição: agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
