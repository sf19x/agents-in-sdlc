---
title: "Lição 1 - Instale o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente uma conversa rápida."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento com agentes. Baseado no GitHub Copilot CLI e integrado ao GitHub, ele permite usar repositórios, branches e pipelines de integração contínua (CI). O aplicativo foi criado para fluxos em que você orienta vários agentes em paralelo, cada um em seu próprio espaço de trabalho isolado, e automatiza tarefas repetitivas. Com o Node.js instalado e a cópia do projeto pronta, agora você instalará o aplicativo e conectará o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar o projeto ao aplicativo pelo repositório do GitHub.
- conhecer o espaço de trabalho e o backlog criado pelo modelo.
- experimentar uma conversa rápida para saber mais sobre o aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará o aplicativo, conectará o projeto e aprenderá a iniciar uma conversa sobre ele.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou um plano pago (Pro, Pro+, Business ou Enterprise). No Copilot Business ou Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

O primeiro passo é instalar o aplicativo. Há versões para Windows, macOS e Linux. Em seguida, autentique-se e adicione o repositório Tailspin Toys.

1. No navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe e instale o aplicativo para sua plataforma seguindo as instruções da página.
3. Abra o aplicativo depois da instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para entrar com uma conta do GitHub.com ou do GHE.com. Se usar o GitHub Enterprise Cloud com residência de dados, selecione **GitHub Enterprise Cloud** e informe o nome do host `*.ghe.com` quando solicitado.

   Se usar o GitHub Enterprise Server, conclua primeiro a integração com sua conta na nuvem. Em seguida, abra **Settings**, selecione **Accounts**, selecione **Add account** e escolha **GitHub Enterprise Server**. Siga as instruções para autorizar o aplicativo no navegador, conforme descrito no [guia de entrada do Enterprise Server][ghes-sign-in].

5. Se o processo de integração oferecer a seleção de repositórios, escolha o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua a integração.
7. Procure seu repositório em **Sessions** na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. Neste layout, as pastas de repositório, como `tailspin-toys`, ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, conheça as áreas da barra lateral:

- **New** — inicie uma nova sessão de conversa com o Copilot.
- **Pull requests** — navegue e filtre pull requests, confira o status de CI e revise PRs usando a integração nativa do GitHub no aplicativo.
- **Issues** — navegue e filtre issues e inicie uma sessão a partir de uma issue.
- **Automations** — tarefas salvas de agentes que são executadas em um cronograma ou sob demanda. Elas ajudam a gerenciar listas de tarefas, manutenção regular do projeto e outras atividades repetitivas. O resumo indica essas automações como próxima etapa, não como outro exercício do workshop.
- **Customize** — adicione recursos ao aplicativo Copilot, como servidores MCP, plugins, skills e outros componentes. Você usará esta área para configurar o Playwright MCP.
- **Chats** — conversas rápidas para perguntas e ideias que não precisam de branch ou espaço de trabalho próprio. Esta seção aparece depois da primeira conversa. Se não estiver visível, selecione **+** ao lado de **Sessions** para iniciar uma conversa.
- **Sessions** — agrupe pastas de repositório e sessões de agentes. Selecione o nome do repositório, como `tailspin-toys`, para abrir a visualização do repositório, ou selecione uma sessão para voltar à conversa. Cada sessão tem seu próprio espaço de trabalho isolado, permitindo executar várias ao mesmo tempo sem conflito. Você iniciará a primeira sessão ao adicionar avaliações por estrelas.

À medida que avançar no workshop, você explorará o espaço de trabalho.

> [!TIP]
> Em caso de dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se não tiver certeza de que é possível, peça orientação ao Copilot.

### Encontre o backlog criado pelo modelo

A maioria dos projetos tem um backlog, e a Tailspin Toys não é exceção. Vamos examinar as issues criadas quando você criou o repositório a partir do modelo.

1. Em **Sessions**, selecione a pasta do repositório Tailspin Toys, não uma sessão individual. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Procure estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma pequena primeira alteração.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimente uma conversa rápida

Uma conversa rápida é uma boa maneira de conhecer o aplicativo. Faça uma pergunta ou explore uma ideia sem criar branch ou worktree — não é necessário iniciar uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir uma conversa. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar sua primeira conversa; **Chats** aparecerá depois.
2. Pergunte como funcionam as sessões do próprio aplicativo:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta. Cada sessão é executada em um worktree Git isolado, o que permite executar vários agentes em paralelo sem conflito entre as alterações. Você pode continuar a conversa ou iniciar outra quando quiser.

## Resumo e próximas etapas

Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto pelo repositório do GitHub.
- conhecer o espaço de trabalho e encontrar o backlog em **Issues**.
- usar uma conversa rápida para fazer uma pergunta pontual.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e exibirá uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
