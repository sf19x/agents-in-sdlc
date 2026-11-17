---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cartões dos jogos e mescle-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conhece o espaço de trabalho e usou uma conversa rápida, é hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e a mesclará no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e aprender como ela é estruturada.
- pedir ao agente que faça uma alteração pequena e focada no projeto.
- revisar a alteração na visualização de diff do espaço de trabalho.
- executar o aplicativo localmente para confirmar a alteração no navegador.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cartões da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada cartão — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio espaço de trabalho isolado. Cada sessão recebe um **worktree Git e uma branch dedicados**, permitindo executar várias sessões ao mesmo tempo — uma para adicionar uma funcionalidade e outra para corrigir um bug, por exemplo — sem conflito entre alterações. As sessões aparecem agrupadas por repositório na barra lateral; selecione uma para alternar para ela.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade das ferramentas** enquanto ele explora e edita arquivos e a lista de **arquivos alterados** com seus diffs.

## Inicie uma sessão e solicite a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar a funcionalidade. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto pelo repositório do GitHub. Agora, crie uma sessão para esse repositório e solicite a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Selecione **+** na parte superior da barra lateral para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Escolha uma **new working tree** e o modo **Interactive** abaixo da caixa de prompt. Use este prompt:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando um worktree novo, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados e cria o código necessário. Você acabou de adicionar uma funcionalidade com o aplicativo Copilot.

## Revise o diff

Toda alteração gerada por IA precisa ser revisada antes da mesclagem, mesmo as pequenas. Vamos examinar as alterações no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código adicionado a `GameCard.astro`, o arquivo principal usado para exibir detalhes dos jogos. A alteração deverá ser semelhante ao trecho a seguir: um bloco pequeno que mostra a avaliação quando ela existe e exibe `No rating yet` quando `starRating` é `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar, mas deve ser relativamente semelhante.

## Verifique as alterações

Revise os resultados das verificações automatizadas do agente antes de abrir o navegador. Confirme que os testes cobrem uma avaliação numérica em `starRating` e a alternativa `null`. Uma dependência ausente ou uma verificação ignorada não significa aprovação; revise qualquer solicitação de instalação antes de autorizá-la.

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que abra o site em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível no aplicativo Copilot. Você explorará canvases personalizados e criará um mais adiante; por enquanto, usaremos o canvas de navegador integrado.

1. Envie este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas do navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Confirme que os cartões dos jogos com avaliação exibem o valor de zero a cinco.
4. Ao terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abra e mescle seu primeiro pull request

Você criou a funcionalidade. Agora, abra um pull request (PR) para mesclar o novo código à base de código existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para autenticar.
3. Se o Copilot informar que a política do repositório exige uma issue correspondente antes de criar o PR, confirme que ele deve seguir esse processo.
4. Aguarde enquanto o Copilot cria o PR.
5. Selecione o indicador **PR** acima do chat para abrir o pull request no painel de revisão. Revise-o conforme necessário.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para mesclar o pull request.

## Resumo e próximas etapas

Você entregou sua primeira alteração usando o aplicativo GitHub Copilot. Especificamente, você:

- iniciou uma sessão de agente e aprendeu como ela é estruturada.
- orientou o agente a fazer uma alteração pequena e focada nos cartões dos jogos.
- revisou a alteração na visualização de diff do espaço de trabalho.
- executou o aplicativo localmente para confirmar a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e mesclou-o explicitamente.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
