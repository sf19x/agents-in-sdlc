---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: pré-requisitos][previous-lesson] · [Próxima lição: modo de agente →][next-lesson]

O contexto é fundamental ao trabalhar com inteligência artificial (IA) generativa. Se uma tarefa precisa ser realizada de uma forma específica — ou se há informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Os [arquivos de instruções][instruction-files] fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo por caminho.
- gerar a primeira fatia de dados para a filtragem (um helper de editoras) usando as instruções atuais.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão.
- confirmar a atualização das instruções e a fatia de filtragem em `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a tendência de mudança de comportamento após atualizar as instruções, não obter exatamente a mesma saída em todos os caracteres.

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

Leia os arquivos de instruções incluídos no repositório pelo editor ou pela interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre fluxos de trabalho do GitHub Actions. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo, que define um glob relativo à raiz do repositório. Por exemplo, ele corresponde a arquivos de teste TypeScript como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Esses caminhos são relativos ao arquivo de instruções, não à raiz do repositório. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Crie uma branch para as alterações

Vamos seguir as práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar as etapas abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; cada etapa indicará o que é relevante.

Para observar o impacto das instruções personalizadas, envie primeiro um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot receba somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na visualização Chat, selecione **Agent** no menu suspenso de agentes para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, normalmente ao helper e aos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele inclua comentários de documentação mesmo sem essa orientação. Tudo bem: o objetivo é observar a melhoria de consistência depois da atualização das instruções.

## Adicione um novo padrão para o repositório

O arquivo `.github/copilot-instructions.md` fornece informações de nível de projeto ao Copilot. Vamos documentar os padrões de codificação para melhorar as sugestões.

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

## Envie o prompt novamente e observe a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie exatamente o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - Cliente do banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editora.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Confirme as instruções e envie a branch

Os arquivos de instruções são gerenciados pelo controle do código-fonte, assim como os demais arquivos do repositório. Vamos confirmar e enviar a branch.

1. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme se as instruções e as alterações do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e confirme a atualização das instruções e todos os arquivos alterados pelo Copilot para criar o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações de suporte, como testes, adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Com a branch enviada, crie um pull request associado à issue de padrões de documentação do backlog. O Copilot pode encontrar a issue, criar um PR que a encerre e, depois, mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub em seu nome. O modelo do projeto já configura as ferramentas do GitHub no workspace. Na primeira vez que o Copilot usar uma delas, o VS Code talvez peça que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona — Model Context Protocol — em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo as instruções e a base do helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Aguarde enquanto o Copilot encontra a issue e cria o PR.
5. Quando solicitado a permitir que o Copilot execute ações no GitHub, revise o comando e selecione **Allow** se apropriado.
6. Depois da criação do PR, peça ao Copilot que o mescle e retorne à branch `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir ações no GitHub e comandos de shell, revise o comando e selecione **Allow** se apropriado.

Agora você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximas etapas

Você explorou como o Copilot usa os arquivos de instruções do projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código proposto adotar o novo padrão.
- confirmar as instruções e a base do helper em `main` para que a próxima lição possa usá-las.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona uma funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
