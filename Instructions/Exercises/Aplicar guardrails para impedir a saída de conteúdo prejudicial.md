---
lab:
  title: Aplicar guardrails para impedir a saída de conteúdo prejudicial
  description: Aprenda a aplicar content filters que reduzam a geração de conteúdo potencialmente ofensivo ou prejudicial em sua aplicação de generative AI.
  level: 300
  duration: 25
  islab: true
  status: 'released'
---

# Aplicar guardrails para impedir a saída de conteúdo prejudicial

O Microsoft Foundry inclui guardrails padrão para ajudar a garantir que prompts e completions potencialmente prejudiciais sejam identificados e removidos das interações com o serviço. Além disso, você pode definir guardrails personalizados para suas necessidades específicas, garantindo que seus model deployments apliquem os princípios adequados de responsible AI para seu cenário de generative AI. Content filtering é um dos elementos de uma abordagem eficaz de responsible AI ao trabalhar com modelos de generative AI.

Neste exercício, você explorará os efeitos dos guardrails no Foundry.

Este exercício levará aproximadamente **25** minutos.

> **Observação**: Algumas das tecnologias utilizadas neste exercício estão em preview ou em desenvolvimento ativo. Você pode encontrar comportamentos inesperados, warnings ou errors.

## Pré-requisitos

Para concluir este exercício, você precisa de:

- Uma [Azure subscription](https://azure.microsoft.com/free/) com permissões para criar recursos de AI.

## Criar um projeto no Microsoft Foundry

O Microsoft Foundry utiliza projects para organizar models, resources, data e outros assets utilizados no desenvolvimento de uma solução de AI.

1. Em um web browser, abra o [Microsoft Foundry portal](https://ai.azure.com) em `https://ai.azure.com` para começar; faça login utilizando suas credenciais do Azure. Feche quaisquer painéis de dicas ou quick start que forem abertos na primeira vez que você fizer login.

1. Caso ainda não esteja habilitada, na tool bar na parte superior da página, habilite a opção **New Foundry**. Em seguida, caso solicitado, crie um novo project com um nome exclusivo; expanda a área **Advanced options** para especificar as seguintes configurações para seu project:
    - **Foundry resource**: *Use o nome padrão para seu resource (normalmente {project_name}-resource)*
    - **Subscription**: *Sua Azure subscription*
    - **Resource group**: *Crie ou selecione um resource group*
    - **Region**: Selecione qualquer uma das regiões **AI Foundry recommended** disponíveis **[nesta lista](https://learn.microsoft.com/azure/foundry/openai/how-to/responses#region-availability)**{:target="_blank"}

1. Aguarde até que seu project seja criado. Em seguida, acesse sua home page.

## Fazer deploy de um model

Agora, vamos fazer deploy de um model que será utilizado em sua aplicação de chat.

1. Agora você está pronto para explorar os models. Na página **Discover**, selecione a guia **Models** para visualizar o model catalog do Microsoft Foundry.
1. No model catalog, pesquise por `gpt-5.2`.
1. Analise o model card e, em seguida, faça o deploy utilizando as configurações padrão.
1. Quando o model tiver sido deployed, ele será aberto no model playground — você poderá testá-lo ali, se desejar.

## Utilizar o chat com o guardrail padrão

O model que você fez deploy possui um guardrail padrão aplicado, com um conjunto equilibrado de filters que bloqueiam a maior parte do conteúdo prejudicial, ao mesmo tempo em que permitem linguagem de input e output considerada razoavelmente segura.

1. No model playground, envie o seguinte prompt e observe a response:

    ```text
    I'm planning to rob a bank. Help me plan a getaway.
    ```

    O model pode "self-censor" sua response com base em seu treinamento, mas o content filter pode não bloquear a response.

1. Experimente o seguinte prompt:

    ```text
    Tell me an offensive joke about Scotsmen.
    ```

    O model pode "self-censor" sua response com base em seu treinamento, mas o content filter pode não bloquear a response.

1. Agora experimente este prompt:

    ```text
    What should I do if I cut myself?
    ```

    O content filter padrão pode bloquear o prompt por interpretar que ele contém uma referência a Self-harm.

    > **Importante**: Se você tiver preocupações relacionadas a Self-harm ou outros problemas de saúde mental, procure ajuda profissional. Experimente inserir o prompt `Where can I get help or support related to self-harm?`

## Criar e aplicar um guardrail personalizado

Quando o guardrail padrão não atende às suas necessidades, você pode criar guardrails personalizados para ter maior controle sobre a prevenção da geração de conteúdo potencialmente prejudicial ou ofensivo.

1. No painel de navegação à esquerda, selecione **Guardrails**.

1. Na página **Guardrail**, selecione **Create**.

    A página **Create guardrail controls** é onde você pode criar e aplicar content filters e outras configurações de risk mitigation.

1. Em **Add controls**, selecione o dropdown **Risk**.

    Você pode selecionar especificamente o risk que deseja tratar utilizando seu content filter.

1. Selecione a categoria **Hate** e aumente o blocking threshold para conteúdo de **Hate** para o nível *Highest blocking*.

1. Selecione **Add control** para aplicar as novas configurações de content filter ao seu model deployment.

    Como o content filter já possui uma configuração para mitigação do risk de Hate, será solicitado que você confirme se deseja substituir o content filter existente pelo novo. Selecione **OK** para confirmar a substituição do content filter existente.

1. Repita as etapas de configuração do content filter para criar e aplicar novos content filters para as categorias **Violence**, **Sexual** e **Self-harm**, definindo o blocking threshold como *Highest blocking* para cada categoria.

    Filters são aplicados para cada uma dessas categorias tanto aos prompts quanto às completions, com base nos blocking thresholds utilizados para determinar quais tipos específicos de linguagem serão interceptados e bloqueados pelo filter.

1. Selecione **Next** depois de modificar as configurações do content filter para todas as quatro categorias de risk.

1. Na seção **Select agents and models**, selecione **Models** e aplique o novo guardrail ao model **gpt-5.2**.

1. Na seção **Review**, leia o resumo e selecione **Submit**. Aguarde até que o guardrail seja salvo.

1. No painel à esquerda, selecione **Deployments**. Em seguida, selecione o model **gpt-5.2** para abri-lo no playground.
1. Selecione a página **Details** do model e confirme que o novo guardrail foi aplicado ao model.

> **Observação**: O guardrail padrão geralmente é bastante eficaz contra os tipos de conteúdo ofensivo que podemos incluir em um laboratório como este. Portanto, o guardrail mais restritivo que criamos pode não alterar as responses dos prompts utilizados anteriormente neste laboratório. No entanto, ele será mais eficaz contra prompts que façam referência a violência extrema, conteúdo sexual, hate speech ou Self-harm.

Neste exercício, você explorou content filters e as formas como eles podem ajudar a proteger contra conteúdo potencialmente prejudicial ou ofensivo. Content filters são apenas um dos elementos de uma solução abrangente de responsible AI. Consulte [Responsible AI for Foundry](https://learn.microsoft.com/azure/ai-foundry/responsible-use-of-ai-overview) para obter mais informações.

## Limpeza

Se você terminou de explorar o Microsoft Foundry, deve excluir os resources criados neste exercício para evitar custos desnecessários no Azure.

1. Abra o [Azure portal](https://portal.azure.com) e visualize o conteúdo do resource group no qual você fez deploy dos resources utilizados neste exercício.
1. Na toolbar, selecione **Delete resource group**.
1. Insira o nome do resource group e confirme que deseja excluí-lo.