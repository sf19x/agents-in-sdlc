---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a publicação, mas você continua responsável por revisar as mudanças e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar a publicação no GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes da mesclagem.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar servidor ou dependência de runtime.

## Peça ao Copilot para preparar a publicação

> [!WARNING]
> Criar um pull request pode confirmar e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Com **Approve all**, não espere uma solicitação de confirmação separada.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare uma implantação do GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e o implante no GitHub Pages depois que as alterações chegarem à branch padrão. Adicione um README que descreva o aplicativo e como executá-lo localmente, caso ainda não exista. Crie um pull request com essas alterações e informe o link e quaisquer configurações ou permissões do Pages necessárias. Não faça a mesclagem, não habilite a mesclagem automática, não altere as configurações do Pages nem implante. Pare e aguarde minha revisão.
```

## Revise o fluxo de trabalho e o pull request

1. Confirme que o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho, os gatilhos e as permissões.
3. Confirme que o fluxo de trabalho implanta a partir da branch padrão, e não de um pull request ainda não mesclado.
4. Solicite as correções necessárias e revise o diff atualizado. Aguarde a aprovação nas verificações obrigatórias do pull request antes de autorizar a mesclagem.

## Autorize a mesclagem e a implantação

> [!WARNING]
> O prompt a seguir autoriza o agente a mesclar o pull request revisado, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e o pull request. As configurações de aprovação podem permitir essas ações sem outra solicitação de confirmação.

Substitua `<PR_NUMBER>` pelo número do pull request que você revisou e envie este prompt separado:

```plaintext
Revisei o pull request <PR_NUMBER> neste repositório e autorizo a mesclagem e a implantação. Habilite o GitHub Pages usando o GitHub Actions, mescle somente esse pull request depois que as verificações obrigatórias forem aprovadas e acompanhe a implantação até que ela seja concluída com êxito. Informe a URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou mesclar, diga exatamente o que preciso aprovar ou fazer; não informe que a tarefa foi concluída até que o site esteja no ar.
```

![Diagrama em três etapas: um pull request do space-quiz mesclado; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz no ar em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a implantação

1. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com êxito.
2. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou mesclar o pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site esteja carregando.

## Resumo e próximas etapas

Agora o Space Quiz está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue em [Lição 13: revisão e próximas etapas][next-lesson].

[next-lesson]: ../13-review/
