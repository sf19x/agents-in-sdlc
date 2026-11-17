---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mudanças maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para trabalhar com o fluxo existente da organização e ajudar a construir as soluções certas da maneira certa. Nesta lição, você começará a seguir um processo de desenvolvimento orientado por agente: usará uma issue para criar uma funcionalidade, verificará se o código está correto e se o comportamento atende às expectativas e, por fim, preparará o trabalho para ser mesclado no projeto.

> [!NOTE]
> Você usará a mesma sessão durante o restante do fluxo de trabalho da funcionalidade. Normalmente, seriam usadas sessões ou PRs diferentes para cada tipo de arquivo, mas faremos esse atalho para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir requisitos no modo **Plan**.
- implementar o recurso no modo **Autopilot**.
- revisar o código.
- validar manualmente a funcionalidade em um canvas de navegador.

Ao continuar o trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com o Model Context Protocol (MCP), criará um agente de garantia de qualidade (QA) e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como combinar categorias precisam ser definidos antes de escrever código. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Adicionar agentes de codificação com inteligência artificial (IA) ao fluxo de desenvolvimento não muda os princípios fundamentais — na verdade, torna-os ainda mais importantes. A maioria dos desenvolvedores segue um processo semelhante:

1. Abra uma issue registrada com os detalhes do trabalho necessário.
2. Crie um plano do que será desenvolvido.
3. Crie e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação do processo de integração contínua, mescle o código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão aplicado ao código escrito manualmente.

## Modos de sessão

O **modo da sessão** controla o nível de autonomia do agente. Você pode selecioná-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano. Você o revisa e aprova antes da execução.
- **Autopilot**: o agente trabalha de forma totalmente autônoma — escreve código, executa testes e itera sem esperar sua orientação.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Antes de começar, confirme que o PR de avaliação por estrelas foi mesclado e que sua cópia local de `main` está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já existe no backlog; não crie outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o `PATH` ou a configuração do gerenciador de versões e feche e reabra o aplicativo completamente. Tente novamente a operação e as verificações com falha. Não reinstale o npm apenas porque o agente não o encontrou.

## Planeje a filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva código. Como você iniciou a sessão pela issue, o Copilot já tem a solicitação de funcionalidade no contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme que ele abrange a filtragem por categoria e editora, controles acessíveis, alterações no acesso a dados e testes. Discuta qualquer comportamento pouco claro, como a combinação de várias categorias ou o resultado quando nenhum jogo corresponde.

O plano deve incluir lint, testes unitários, testes de ponta a ponta (E2E) e verificação de tipos com as ferramentas existentes do projeto. Mantenha o escopo em implementar e testar a filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações ao plano antes de aprová-lo e guarde a URL da issue e quaisquer esclarecimentos acordados para a validação posterior.

## Aprove explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confira o modo ativo, em vez de presumir que a aprovação o alterou. Confirme que o indicador de modo mostra **Autopilot**.

O Copilot começará a implementação, seguindo o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revise e verifique a implementação

Como qualquer outro código, o código gerado precisa ser revisado antes da mesclagem. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Inspecione os resultados do lint, dos testes unitários, dos testes E2E e da verificação de tipos. Uma verificação ignorada não significa aprovação.
4. Corrija falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma prévia e pode reutilizar um servidor local. Confirme que o servidor testado pertence a este worktree, não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos pedir ao Copilot que inicie o aplicativo e abra o site em um canvas de navegador.

1. Envie este prompt:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme os resultados. Limpe o filtro, selecione uma editora e confirme os resultados correspondentes.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado definido no plano.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Ao terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem mescle o PR de filtragem. Mantenha esta sessão, o worktree e a branch para as Lições 4 a 8. Em seguida, aplique instruções personalizadas ao código de filtragem e adicione melhorias à skill e ao agente de QA antes de criar e mesclar o PR combinado na Lição 8. Depois de parar o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximas etapas

Você usou diferentes modos de agente para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu requisitos no modo **Plan**.
- implementou o recurso no modo **Autopilot**.
- revisou o código.
- validou manualmente a funcionalidade em um canvas de navegador.

Em seguida, você verá como garantir que o código siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
