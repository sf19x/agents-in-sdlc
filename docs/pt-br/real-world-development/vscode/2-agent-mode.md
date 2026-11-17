---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e tarefas como executar testes. O fluxo normal de desenvolvimento pode envolver localizar todos os arquivos necessários, fazer alterações, executar testes, depurar, descobrir qual arquivo ficou de fora e repetir o processo. O modo de agente do Copilot pode ajudar nesse trabalho.

O modo de agente atua com mais autonomia no ambiente de desenvolvimento integrado (IDE). Ele explora a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos usá-lo para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente na nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, com ciclos rápidos de feedback e interação. O agente na nuvem funciona de forma assíncrona, como um colega da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo de agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site para conhecer o comportamento atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele lista jogos e exibe seus detalhes. O site é um único aplicativo Astro que renderiza páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local pelo frontmatter usando o Drizzle ORM; não há uma API de backend separada nem uma estrutura de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno `<script>` Astro com escopo próprio e APIs padrão do DOM.

### Inicie o site

O comando `npm run dev` inicia o único servidor de desenvolvimento Astro. Execute-o no GitHub Codespace:

1. Volte ao codespace. Você deverá estar na branch `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, deverá aparecer uma mensagem semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site aberto, explore suas funcionalidades:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de continuar, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; cada etapa indicará o que é relevante.

A implementação inicial do site funciona, mas queremos adicionar recursos. Vamos começar revisando o backlog de issues criado automaticamente quando você configurou o projeto a partir do modelo.

1. Na visualização Chat, selecione **Agent** no menu suspenso de agentes. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criar o workshop para tentar manter as sugestões relativamente consistentes e oferecer uma boa experiência. Como os modelos de linguagem grandes (LLMs) são probabilísticos, as sugestões podem ser diferentes das mostradas aqui. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem grandes (LLMs), o Copilot poderá usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot que mostre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe como o Copilot também prioriza os itens de acordo com o que considera mais útil para o usuário.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto. O arquivo [de instruções da interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) explica como abordar a adição de funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas sobre Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso facilita a reutilização, a divisão de conjuntos complexos em partes menores e o uso de exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, será necessário atualizar pelo menos três partes do aplicativo:

- adicionar ou refinar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página que lista os jogos para incluir a interface de filtragem.

Os testes também devem ser executados e aprovados antes da mesclagem. O modo de agente do Copilot pode realizar essas tarefas.

1. Continue na conversa atual do Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela de um exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu suspenso de agentes.

   ![Captura de tela do seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade da issue relacionada no backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos da funcionalidade, da camada de dados, da interface e dos testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você verá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. Isso é esperado: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está aberto e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximas etapas

Neste exercício, você explorou como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados.
- o modo de agente explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar as alterações e os testes gerados antes de mesclá-los à base de código.

Agora, vamos [testar a funcionalidade com o servidor Playwright MCP][next-lesson] e abrir um pull request.

### Exercício extra: implemente a paginação

À medida que a lista de jogos crescer, será necessário implementar paginação. Use as habilidades deste exercício e peça ao Copilot que atualize o site. Considere as práticas recomendadas e os arquivos de instruções existentes, decida se o usuário poderá escolher o tamanho da página ou se ele será fixo e forneça orientações suficientes no prompt. Talvez seja necessário iterar e fornecer mais contexto; esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usá-los][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
