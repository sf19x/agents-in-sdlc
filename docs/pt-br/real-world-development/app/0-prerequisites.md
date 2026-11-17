---
title: "Lição 0 - Pré-requisitos"
description: "Prepare-se para as lições do aplicativo GitHub Copilot: instale o Node.js para o projeto Tailspin Toys e crie sua própria cópia do repositório a partir do modelo."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O aplicativo GitHub Copilot é um aplicativo para desktop que funciona como ponto central para o Copilot e o GitHub. Ele oferece acesso rápido a issues e pull requests e permite desenvolver com o GitHub Copilot. Durante este workshop, você trabalhará localmente com o aplicativo Tailspin Toys, criado com Astro, usando o aplicativo GitHub Copilot. Primeiro, confirme se o Node.js está instalado localmente e, em seguida, instale o aplicativo.

Nesta lição, você vai:

- instalar o Node.js para executar os testes do projeto no computador.
- criar sua própria cópia do projeto Tailspin Toys a partir do modelo.

## Instale o Node.js

Em várias lições, você pedirá a um agente que crie funcionalidades e execute o conjunto de testes do Tailspin Toys localmente. Para isso, é necessário o [Node.js][nodejs], o único runtime exigido pelo projeto. Instale a versão **LTS** atual.

A opção mais simples em todas as plataformas é usar o instalador oficial:

1. Abra uma janela de terminal no sistema operacional. Você pode usar o Windows Terminal, o Terminal do macOS ou o aplicativo que costuma usar.
2. Verifique as versões instaladas do Node.js e do npm:

    ```shell
    node --version
    npm --version
    ```

3. Se os dois comandos forem executados e o Node.js atender aos requisitos do README e do `package.json` do projeto, passe para a próxima seção.

> [!TIP]
> Conclua estas etapas somente se o Node.js não estiver instalado ou precisar ser atualizado.

4. Abra a [página de download do Node.js][node-download].
5. Baixe a versão **LTS** para o seu sistema operacional.
6. Execute o instalador e aceite as opções padrão. No Windows, mantenha selecionada a opção **Add to PATH**.
7. Depois da instalação, abra uma nova janela de terminal.
8. Confirme a instalação na nova janela executando:

    ```bash
    node --version
    npm --version
    ```

9. As versões instaladas do Node.js e do npm devem aparecer.

> [!IMPORTANT]
> O agente do aplicativo Copilot também precisa executar `node --version` e `npm --version`. O sucesso no terminal habitual não garante que os comandos estejam disponíveis para o aplicativo. Feche o aplicativo completamente e abra-o novamente depois de instalar o Node.js ou alterar o `PATH`. Se algum comando ainda falhar na sessão, siga as [etapas para solucionar problemas do ambiente da sessão][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree também precisa das dependências do projeto e do Chromium do Playwright para verificações de ponta a ponta (E2E). Siga o README do repositório Tailspin Toys ao preparar uma worktree e revise as solicitações de instalação antes de aprová-las.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório modelo][template-repository]. O novo repositório conterá todos os arquivos necessários para o laboratório; você o conectará ao aplicativo na próxima lição.

1. Em uma nova janela do navegador, acesse o repositório do GitHub deste laboratório: `https://github.com/github-samples/tailspin-toys`.
2. Crie sua cópia selecionando **Use this template** na página do repositório e, em seguida, **Create a new repository**.

    ![Botão Use this template com a opção Create a new repository selecionada no menu suspenso](../../../_images/app-0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Formulário Create a new repository com github-samples/tailspin-toys definido como modelo e o nome do repositório preenchido](../../../_images/app-0-create-repository.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante.

> [!NOTE]
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop; não precisa criar nenhuma.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, uma skill `quality-checks` e uma extensão de canvas existente. Você personalizará a skill e criará um agente de QA durante o workshop. Se usar uma cópia mais antiga, confirme com a pessoa que facilita o workshop se ela contém os arquivos necessários.

## Resumo e próximas etapas

O ambiente está pronto. Nesta lição, você:

- instalou o Node.js para que o projeto possa ser compilado e testado no computador.
- criou sua própria cópia do repositório Tailspin Toys a partir do modelo.

Em seguida, você [instalará o aplicativo GitHub Copilot][next-lesson], conectará o repositório criado e conhecerá o espaço de trabalho.

## Recursos

- [Baixar o Node.js][node-download]
- [Criar um repositório a partir de um modelo][template-repository]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicie-uma-sessao-a-partir-da-issue
