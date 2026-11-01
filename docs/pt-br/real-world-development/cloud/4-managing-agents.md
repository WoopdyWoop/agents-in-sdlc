---
title: "Exercício 4 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas disponíveis para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas do agente com pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria uma boa oportunidade para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar alternância entre modos, pareceu natural incluir esse recurso. Você orientará o agente para garantir que ele implemente tanto o modo claro quanto o modo de alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos verificar o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você verá as tarefas atribuídas ao Copilot, incluindo:
    - atualizar a documentação da base de código.
    - gerar APIs para modificar jogos.
    - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações realizadas pelo Copilot, que podem incluir:
    - obter o código do repositório.
    - criar o ambiente de trabalho do Copilot.
    - configurar servidores MCP.
    - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exatas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem utilizada.

4. Observe também o painel de pull requests à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à adição do modo de alto contraste. O título exato varia de acordo com o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar uma sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo de trabalho depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Como essa tarefa também levará alguns minutos, aproveite para fazer uma pausa e refletir sobre o que você aprendeu até aqui.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, o ponto central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Nesse painel, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento para ajustar o escopo ou fornecer outras orientações.

Você explorou estes tópicos:

- usou o painel central e a página de agentes para acompanhar as tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: agentes personalizados][previous-lesson] | [Próxima lição: iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
