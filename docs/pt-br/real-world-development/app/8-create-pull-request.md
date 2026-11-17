---
title: "Lição 8 - Crie e mescle o PR da funcionalidade"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, autorize o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora, revise tudo em conjunto e abra um pull request. Você mesclou o PR de avaliação por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluiremos todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar o PR completo da funcionalidade e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar a mesclagem do PR.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização da mesclagem sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request no aplicativo GitHub Copilot. Quando habilitado, a sessão do aplicativo lê o PR e resolve os bloqueios — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e mescla o PR assim que o GitHub permite. O processo é executado em segundo plano, continua após reiniciar o aplicativo e é desativado quando o PR é mesclado.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade, mas você ainda precisa autorizar explicitamente a edição do código e a mesclagem. Revise as ações permitidas e o trabalho antes de conceder permissão para mesclar.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agente para escolher **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia a operação.

O Agent Merge começará a trabalhar. Ele poderá:

- criar o pull request com título e descrição.
- se a sessão tiver sido iniciada a partir de uma issue, referenciá-la no corpo da descrição.
- fazer rebase ou resolver possíveis conflitos de mesclagem com a branch de destino.
- acompanhar o processo de CI para garantir que todas as verificações sejam aprovadas.
- monitorar comentários de outros desenvolvedores e da revisão de código do Copilot e fazer alterações para resolvê-los.
- mesclar o PR automaticamente depois que tudo for aprovado, se você autorizar essa ação.

Agora, vamos permitir que o Agent Merge mescle o PR depois que todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para mesclar.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme que **Merge pull request** está marcado.
8. Peça explicitamente ao agente que mescle:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios informados. Confirme que o estado do PR é **Merged** antes de continuar. Ter habilitado a permissão de mesclagem ou um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximas etapas

Você automatizou várias etapas do processo de desenvolvimento: geração de código, teste e validação e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou o PR completo da funcionalidade e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou a mesclagem do PR.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma maneira mais rica de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
