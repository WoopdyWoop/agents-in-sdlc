---
title: "Exercício 3 - Teste sua funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: modo de agente][previous-lesson] · [Próxima lição: agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ela realmente funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar a funcionalidade. Em seguida, publique sua ramificação e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis ao Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que use esse servidor para testar manualmente a funcionalidade de filtragem em um navegador.
- publicar sua ramificação e abrir um pull request do trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Copilot faz isso com o Model Context Protocol (MCP), um padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama dos componentes internos do modo de agente e de sua interação com o contexto, o LLM e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas por meio de recursos e realizar ações em seu nome por meio de ferramentas.

Essas ferramentas e recursos são acessados por um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais em pacotes npm. Cada servidor MCP oferece ao agente um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. Ele permite que o agente de IA crie repositórios, atualize os existentes e gerencie issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright. Ele permite que o agente de IA navegue por páginas Web, preencha formulários e selecione botões.

Há muitos outros servidores MCP disponíveis que oferecem acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e a contribuição para o ecossistema.

> [!CAUTION]
> Por segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do modelo do projeto, por isso o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code conecta-se a ele por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor MCP do Playwright

Agora, você adicionará outro servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, ideal para testar sua funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` orienta o Playwright a executar o navegador sem uma janela visível. Isso é necessário em um codespace, que não tem uma área de trabalho para exibir a janela.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e pedirá que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para que ele possa iniciar.

Não é obrigatório iniciá-lo manualmente. Se você pular essa etapa, o VS Code iniciará o servidor e pedirá confirmação quando o Copilot precisar de suas ferramentas na próxima etapa.

## Teste a funcionalidade de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] permite que o Copilot controle um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que encontrou. Essa é uma maneira rápida de confirmar se a funcionalidade funciona como esperado sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados (botões, links, itens de lista) como faz uma tecnologia assistiva. Uma verificação funcional rápida também permite uma checagem básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar a funcionalidade de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo não parecer certo, faça perguntas de acompanhamento ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> Para este teste, o aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a ramificação e crie um pull request

Agora que confirmou que a funcionalidade funciona, você pode abrir um pull request (PR) para que a equipe a revise. Primeiro, publique a ramificação `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a ramificação ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, incluindo o github.com e a interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR. Ele pode encontrar a issue relacionada e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão nova.
2. Confirme se **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna as informações e execute as operações necessárias. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar-se. Siga as instruções para autorizar o acesso.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link exibido pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Parabéns. Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis ao Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou esse servidor para testar manualmente a funcionalidade de filtragem em um navegador antes de publicá-la.
- publicou sua ramificação e abriu um pull request do trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma das maneiras de ampliar os recursos do Copilot. Em seguida, você vai [criar um agente personalizado][next-lesson] para facilitar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
