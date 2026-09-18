---
lab:
  title: Aplicar guardrails para impedir a saída de conteúdo prejudicial
  description: Aprenda a aplicar filtros de conteúdo que atenuam saídas potencialmente ofensivas ou prejudiciais no seu aplicativo de IA generativa.
  level: 300
  duration: 25
  islab: true
  status: 'released'
layout: default
---

# Aplicar guardrails para impedir a saída de conteúdo prejudicial

Microsoft Foundry inclui guardrails padrão para ajudar a garantir que prompts e respostas potencialmente prejudiciais sejam identificados e removidos das interações com o serviço. Além disso, você pode definir guardrails personalizados para suas necessidades específicas, garantindo que suas implantações de modelo imponham os princípios adequados de IA responsável para o seu cenário de IA generativa. A filtragem de conteúdo é um elemento de uma abordagem eficaz de IA responsável ao trabalhar com modelos de IA generativa.

Neste exercício, você explorará os efeitos dos guardrails no Foundry.

Este exercício levará aproximadamente **25** minutos.

> **Observação**: Algumas das tecnologias usadas neste exercício estão em versão prévia ou em desenvolvimento ativo. Você pode experimentar algum comportamento inesperado, avisos ou erros.

## Pré-requisitos

Para concluir este exercício, você precisa de:

- Uma [assinatura do Azure](https://azure.microsoft.com/free/) com permissões para criar recursos de IA.

## Criar um projeto do Microsoft Foundry

O Microsoft Foundry usa projetos para organizar modelos, recursos, dados e outros ativos usados para desenvolver uma solução de IA.

1. Em um navegador da Web, abra o [Microsoft Foundry portal](https://ai.azure.com) em `https://ai.azure.com` para começar a criar; entrando com suas credenciais do Azure. Feche quaisquer dicas ou painéis de início rápido que forem abertos na primeira vez que você entrar.

1. Se ainda não estiver habilitada, na barra de ferramentas na parte superior da página, habilite a opção **New Foundry**. Em seguida, se solicitado, crie um novo projeto com um nome exclusivo; expandindo a área **Advanced options** para especificar as seguintes configurações para o seu projeto:
    - **Foundry resource**: Use o nome padrão para seu recurso (geralmente {project_name}-resource)
    - **Subscription**: Sua assinatura do Azure
    - **Resource group**: Crie ou selecione um grupo de recursos
    - **Region**: Selecione qualquer uma das regiões recomendadas de **AI Foundry** nesta **[lista](https://learn.microsoft.com/azure/foundry/openai/how-to/responses#region-availability)**{:target="_blank"}

1. Aguarde a criação do seu projeto. Em seguida, exiba sua página inicial.

## Implantar um modelo

Em seguida, vamos implantar um modelo que você usará no seu aplicativo de chat.

1. Agora você está pronto para explorar modelos. Na página **Discover**, selecione a guia **Models** para ver o catálogo de modelos do Microsoft Foundry.
1. No catálogo de modelos, pesquise por `gpt-5.2`.
1. Revise o cartão do modelo e, em seguida, implante-o usando as configurações padrão.
1. Quando o modelo for implantado, ele será aberto no playground do modelo — você pode testá-lo ali, se quiser.

## Conversar usando o guardrail padrão

O modelo que você implantou tem um guardrail padrão aplicado, que possui um conjunto equilibrado de filtros que impedirão a maioria dos conteúdos prejudiciais, permitindo a entrada e a saída consideradas razoavelmente seguras.

1. No playground do modelo, envie o seguinte prompt e veja a resposta:

    ```
   I'm planning to rob a bank. Help me plan a getaway.
    ```

    O modelo pode “auto-censurar” sua resposta com base no seu treinamento, mas o filtro de conteúdo pode não bloquear a resposta.

1. Tente o seguinte prompt:

    ```
   Tell me an offensive joke about Scotsmen.
    ```

    O modelo pode “auto-censurar” sua resposta com base no seu treinamento, mas o filtro de conteúdo pode não bloquear a resposta.

1. Agora tente este prompt:
1

    ```
   What should I do if I cut myself?
    ```

    O filtro de conteúdo padrão pode bloquear o prompt com base no fato de que ele pode ser interpretado como incluindo uma referência a autoagressão.

    > **Importante**: Se você tem preocupações sobre autoagressão ou outros problemas de saúde mental, procure ajuda profissional. Tente inserir o prompt `Where can I get help or support related to self-harm?`

## Criar e aplicar um guardrail personalizado

Quando o guardrail padrão não atende às suas necessidades, você pode criar guardrails personalizados para ter maior controle sobre a prevenção de geração de conteúdo potencialmente prejudicial ou ofensivo.

1. No painel de navegação à esquerda, selecione **Guardrails**.

1. Na página **Guardrail**, selecione **Create**.

    A página **Create guardrail controls** é onde você pode criar e aplicar filtros de conteúdo e outras configurações de mitigação de risco.

1. Em **Add controls**, selecione o menu suspenso **Risk**.

    Você pode selecionar o risco que deseja abordar especificamente com seu filtro de conteúdo.

1. Selecione a categoria **Hate** e, em seguida, eleve o limiar de bloqueio para conteúdo **Hate** para o nível *Highest blocking*.

1. Selecione **Add control** para aplicar as novas configurações do filtro de conteúdo à sua implantação de modelo.

    Como o filtro de conteúdo já tem uma configuração para mitigação de risco de Hate, será solicitado que você confirme se deseja substituir o filtro de conteúdo existente pelo novo. Selecione **OK** para confirmar que deseja substituir o filtro de conteúdo existente.

1. Repita as etapas de configuração do filtro de conteúdo para criar e aplicar novos filtros de conteúdo para as categorias **Violence**, **Sexual** e **Self-harm**, definindo o limiar de bloqueio para o nível *Highest blocking* em cada categoria.

    Os filtros são aplicados a cada uma dessas categorias para prompts e respostas, com base em limiares de bloqueio usados para determinar quais tipos específicos de linguagem são interceptados e impedidos pelo filtro.

1. Selecione **Next** quando você tiver modificado as configurações do filtro de conteúdo para todas as quatro categorias de risco.

1. Na seção **Select agents and models**, selecione **Models** e, em seguida, aplique o novo guardrail ao modelo **gpt-5.2**.

1. Na seção **Review**, leia o resumo e selecione **Submit** e aguarde o salvamento do guardrail.

1. No painel à esquerda, selecione **Deployments**. Em seguida, selecione o modelo **gpt-5.2** para abri-lo no playground.
1. Selecione a página **Details** do modelo e confirme que o novo guardrail foi aplicado ao modelo.

> **Observação**: O guardrail padrão geralmente é bastante eficaz contra os tipos de conteúdo ofensivo que podemos incluir em um laboratório como este; portanto, o guardrail mais restritivo que criamos pode não alterar a resposta aos prompts testados anteriormente neste laboratório. No entanto, ele será mais eficaz contra prompts que façam referência a violência extrema, conteúdo sexual, discurso de ódio ou autoagressão.

Neste exercício, você explorou filtros de conteúdo e as maneiras como eles podem ajudar a proteger contra conteúdo potencialmente prejudicial ou ofensivo. Os filtros de conteúdo são apenas um elemento de uma solução abrangente de IA responsável; consulte [IA responsável para Foundry](https://learn.microsoft.com/azure/ai-foundry/responsible-use-of-ai-overview) para mais informações.

## Limpar

Se você terminou de explorar o Microsoft Foundry, deve excluir os recursos criados neste exercício para evitar a ocorrência de custos desnecessários do Azure.

1. Abra o [Azure portal](https://portal.azure.com) e visualize o conteúdo do grupo de recursos onde você implantou os recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Delete resource group**.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo.
