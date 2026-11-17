---
title: "Exercício 1 - Instruções personalizadas (agente na nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: pré-requisitos][previous-lesson] · [Próxima lição: agente na nuvem do Copilot →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma forma específica — ou se há informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Os [arquivos de instruções][instruction-files] orientam o Copilot não apenas sobre *o que* fazer, mas também *como* fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo por caminho.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos ambientes do VS Code e do CLI, aqui você não executará um prompt *antes e depois*. O agente na nuvem do Copilot trabalha de forma assíncrona com issues do GitHub, o que torna mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests do agente.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o tema escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação TSDoc (TypeScript documentation).
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Você pode especificar convenções de codificação, bibliotecas e os tipos de comentários a incluir no código. As instruções podem se aplicar a todo o repositório ou a tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um arquivo único enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações de nível de projeto relevantes para a maioria das solicitações, como a pilha tecnológica, uma visão geral do produto, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`, arquivos que podem fornecer diretrizes para linguagens ou tipos de arquivo específicos, como TypeScript ou Astro, ou para tarefas como criar componentes de interface ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente na nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [aceita arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste exercício, mas os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações de nível de projeto, como a descrição do que está sendo criado, a estrutura do projeto e os padrões globais de codificação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não existe uma única maneira certa de criar arquivos de instruções, assim como não existe uma única forma de usar IA. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto, você encontrará instruções para tarefas como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface apresenta isso de um jeito diferente — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição sobre cada interface indicará quando esse recurso for relevante.
>
> Para encontrar modelos e pontos de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizados do projeto

Leia os arquivos de instruções do repositório pela interface Web do GitHub ou por um codespace, se já houver um aberto.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre fluxos de trabalho do GitHub Actions. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo, que define um glob relativo à raiz do repositório para determinar a quais arquivos as instruções se aplicam. Neste caso, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Esses caminhos são relativos ao arquivo de instruções, não à raiz do repositório. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicione um padrão para todo o repositório

Esta será a única edição feita neste exercício: uma regra para que a documentação no código use comentários TSDoc e um cabeçalho de comentário no nível do arquivo. O agente na nuvem aplicará essa regra às issues atribuídas mais adiante.

Antes de editar, crie uma branch. O agente na nuvem lerá as instruções da branch de destino da issue, mas você confirmará suas alterações de forma organizada:

1. No terminal do codespace, crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

O arquivo `.github/copilot-instructions.md` fornece informações de nível de projeto ao Copilot. Vamos documentar os padrões para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela descreve os padrões de codificação, mas ainda não inclui documentação no código.
3. Adicione as linhas Markdown a seguir logo depois dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ficar no nível do repositório em `.github/copilot-instructions.md` ou em arquivos `*.instructions.md` específicos de linguagens, tipos de arquivo ou tarefas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação.

## Confirme, envie e mescle a atualização das instruções

O agente na nuvem lê as instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, as instruções precisam chegar a `main` para que o agente possa usá-las.

1. Prepare e confirme a alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e mescle-o. O agente na nuvem poderá usar essas instruções nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main`, ignore as etapas da branch e confirme a alteração diretamente nessa branch. A etapa de branch foi incluída para refletir o processo usado em um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot usa os arquivos de instruções do projeto e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão aplicado nos pull requests do agente na nuvem durante o restante deste ambiente.

Em seguida, você [atribuirá sua primeira issue ao agente na nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
