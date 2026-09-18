---
lab:
  title: Fazer ajuste fino de um modelo de linguagem
  description: Saiba como usar seus próprios dados de treinamento para realizar ajuste fino de um modelo e personalizar seu comportamento.
  level: 300
  duration: 90
  islab: true
  status: 'released'
layout: default
---

# Fazer ajuste fino de um modelo de linguagem

Quando você quer que um modelo de linguagem se comporte de determinada maneira, pode usar engenharia de prompt para definir o comportamento desejado. Quando você quer melhorar a consistência desse comportamento, pode optar por fazer o ajuste fino de um modelo, comparando-o com sua abordagem de engenharia de prompt para avaliar qual método atende melhor às suas necessidades.

Neste exercício, você fará o ajuste fino de um modelo de linguagem com o Microsoft Foundry que você deseja usar para um cenário de aplicativo de chat personalizado. Você comparará o modelo ajustado com um modelo base para avaliar se o modelo ajustado atende melhor às suas necessidades.

Imagine que você trabalha para uma agência de viagens e está desenvolvendo um aplicativo de chat para ajudar pessoas a planejarem suas férias. O objetivo é criar um chat simples e inspirador que sugira destinos e atividades com um tom de conversa amigável e consistente.

Este exercício levará aproximadamente **90** minutos\*.

> \* **Observação**: Esse tempo é uma estimativa com base na experiência média. O ajuste fino depende de recursos de infraestrutura de nuvem, que podem levar um tempo variável para serem provisionados, dependendo da capacidade do datacenter e da demanda simultânea. Algumas atividades neste exercício podem levar <u>muito</u> tempo para serem concluídas e exigem paciência. Se as coisas estiverem demorando, considere revisar a [documentação de ajuste fino do Microsoft Foundry](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/fine-tuning?view=foundry) ou fazer uma pausa. É possível que alguns processos esgotem o tempo limite ou pareçam executar indefinidamente. Algumas das tecnologias usadas neste exercício estão em versão prévia ou em desenvolvimento ativo. Você pode experimentar comportamentos inesperados, avisos ou erros.

## Pré-requisitos

Para concluir este exercício, você precisa de:

- Uma [assinatura do Azure](https://azure.microsoft.com/free/) com permissões para criar recursos de IA.

## Criar um projeto do Microsoft Foundry

O Microsoft Foundry usa projetos para organizar modelos, recursos, dados e outros ativos usados para desenvolver uma solução de IA.

1. Em um navegador da Web, abra o [portal do Microsoft Foundry](https://ai.azure.com) em `https://ai.azure.com` para começar a criar; entrando com suas credenciais do Azure. Feche quaisquer dicas ou painéis de início rápido que forem abertos na primeira vez em que você entrar.

1. Se ainda não estiver habilitada, na barra de ferramentas na parte superior da página, habilite a opção **New Foundry**. Em seguida, se solicitado, crie um novo projeto com um nome exclusivo; expandindo a área **Opções avançadas** para especificar as seguintes configurações para o seu projeto:
    - **Recurso do Foundry**: *Use o nome padrão para o seu recurso (geralmente {project_name}-resource)*
    - **Assinatura**: *Sua assinatura do Azure*
    - **Grupo de recursos**: *Crie ou selecione um grupo de recursos*
    - **Região**: *Selecione uma das seguintes regiões*:\*
        - North Central US
        - Sweden Central

    > \* No momento da redação, essas regiões dão suporte a ajuste fino para modelos gpt-5. Verifique a [página de modelos](https://learn.microsoft.com/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure?&pivots=azure-openai#fine-tuning-models) para saber a disponibilidade mais recente por região.

1. Aguarde a criação do seu projeto. Em seguida, veja a página inicial dele.

## Implantar um modelo

Em seguida, vamos implantar um modelo que você usará para obter uma linha de base de desempenho.

1. Agora você está pronto para explorar modelos. Na página **Descobrir**, selecione a guia **Modelos** para ver o catálogo de modelos do Microsoft Foundry.
1. No catálogo de modelos, pesquise por `gpt-5`.
1. Revise o card do modelo e, em seguida, implante-o usando as configurações padrão.
1. Quando o modelo tiver sido implantado, ele será aberto no playground do modelo.

## Fazer ajuste fino de um modelo

Como o ajuste fino de um modelo leva algum tempo para ser concluído, você iniciará o trabalho de ajuste fino agora e voltará a ele depois de explorar o modelo base gpt-5 que você já implantou.

1. Baixe o [conjunto de dados de treinamento](https://microsoftlearning.github.io/mslearn-ai-studio/data/travel-finetune-hotel.jsonl) em `https://microsoftlearning.github.io/mslearn-ai-studio/data/travel-finetune-hotel.jsonl` e salve-o localmente como um arquivo JSONL.

    > **Observação**: Seu dispositivo pode, por padrão, salvar o arquivo como .txt. Selecione todos os arquivos e remova o sufixo .txt para garantir que você está salvando o arquivo como JSONL.

1. No portal do Foundry, enquanto visualiza o playground do modelo, no painel de navegação à esquerda, selecione **Ajuste fino**.
1. Selecione o botão **Ajuste fino** no canto superior direito e, em seguida, configure o trabalho de ajuste fino com as seguintes configurações:
    - **Modelo base**: Selecione **gpt-5**
    - **Método de personalização**: Supervisionado
    - **Tipo de treinamento**: Padrão
    - **Dados de treinamento**: Selecione **Carregar novo conjunto de dados** e carregue o arquivo .jsonl baixado anteriormente.
    - **Sufixo**: `ft-travel`
    - **Implantar modelo automaticamente após a conclusão do trabalho**: Selecionado
    - **Tipo de implantação**: Developer
    - *Deixe os demais hiperparâmetros em seus padrões*
1. Selecione **Enviar** para iniciar o trabalho de ajuste fino. Pode levar algum tempo para ser concluído. Você pode continuar com a próxima seção do exercício enquanto aguarda.

> **Observação**: O ajuste fino e a implantação podem levar um tempo significativo (60 minutos ou mais), portanto, talvez seja necessário verificar periodicamente. Você pode ver mais detalhes do progresso até o momento selecionando o trabalho de ajuste fino e visualizando a guia **Monitor**.

## Conversar com um modelo base

Enquanto você aguarda a conclusão do trabalho de ajuste fino, vamos conversar com um modelo de base *gpt-5* para avaliar seu desempenho.

1. No painel à esquerda, selecione **Implantações** e, em seguida, selecione o modelo base **gpt-5** que você implantou anteriormente.
1. No painel de chat, insira o prompt `What can you do?` e veja a resposta.

    As respostas podem ser bastante genéricas. Lembre-se de que queremos criar um aplicativo de chat que inspire as pessoas a viajar.

1. Altere as **Instruções** do modelo para o seguinte prompt:

    ```
   You are an AI assistant that helps people plan their travel.
    ```

1. Na janela de chat, insira novamente a consulta `What can you do?` e veja a resposta.

    Como resposta, o assistente pode dizer que pode ajudar você a reservar voos, hotéis e carros para a sua viagem. Você quer evitar esse comportamento.

1. No campo **Instruções**, insira um novo prompt:

    ```
   You are an AI travel assistant that helps people plan their trips. Your objective is to offer support for travel-related inquiries, such as visa requirements, weather forecasts, local attractions, and cultural norms.
   You should not provide any hotel, flight, rental car or restaurant recommendations.
   Ask engaging questions to help someone plan their trip and think about what they want to do on their holiday.
    ```

1. Continue testando o modelo para revisar seu comportamento. Por exemplo, faça as seguintes perguntas e observe as respostas do modelo, prestando atenção especial ao tom e ao estilo de escrita que o modelo usa para responder:

    `Where in Rome should I stay?`

    `I'm mostly there for the food. Where should I stay to be within walking distance of affordable restaurants?`

    `What are some local delicacies I should try?`

    `When is the best time of year to visit in terms of the weather?`

    `What's the best way to get around the city?`

## Examinar o arquivo de treinamento

O modelo base parece funcionar bem o suficiente, mas você pode estar buscando um estilo de conversa específico para seu aplicativo de IA generativa. Os dados de treinamento usados para o ajuste fino oferecem a chance de criar exemplos explícitos dos tipos de resposta que você deseja.

1. Abra o arquivo JSONL que você baixou anteriormente (você pode abri-lo em qualquer editor de texto)
1. Examine a lista de documentos JSON no arquivo de dados de treinamento. O primeiro deve ser semelhante a este (formatado para legibilidade):

    ```json
    {"messages": [
        {"role": "system", "content": "You are an AI travel assistant that helps people plan their trips. Your objective is to offer support for travel-related inquiries, such as visa requirements, weather forecasts, local attractions, and cultural norms. You should not provide any hotel, flight, rental car or restaurant recommendations. Ask engaging questions to help someone plan their trip and think about what they want to do on their holiday."},
        {"role": "user", "content": "What's a must-see in Paris?"},
        {"role": "assistant", "content": "Oh la la! You simply must twirl around the Eiffel Tower and snap a chic selfie! After that, consider visiting the Louvre Museum to see the Mona Lisa and other masterpieces. What type of attractions are you most interested in?"}
        ]}
    ```

    Cada interação de exemplo na lista inclui a mesma mensagem de sistema que você testou com o modelo base, um prompt do usuário relacionado a uma consulta de viagem e uma resposta. O estilo das respostas nos dados de treinamento ajudará o modelo ajustado a aprender como deve responder.

## Testar o modelo ajustado

Quando seu modelo ajustado estiver pronto, você poderá testá-lo como testou seu modelo base implantado.

1. No painel à esquerda, selecione **Ajuste fino** e revise o status do trabalho de ajuste fino que você iniciou anteriormente.
1. Selecione o trabalho para ver seus detalhes. Você pode usar a guia **Logs** para revisar as tarefas de ajuste fino que foram executadas até o momento.
1. Quando o ajuste fino for concluído e o modelo tiver sido implantado automaticamente, veja a página **Implantações** para verificar se ele está listado.

    > **Dica**: Se a implantação automática falhar, selecione o trabalho de ajuste fino concluído e implante o modelo a partir de lá.
1. Selecione o modelo ajustado para abri-lo no playground do modelo.
1. Atualize as **Instruções** para serem as mesmas que você testou com o modelo base:

    ```
   You are an AI travel assistant that helps people plan their trips. Your objective is to offer support for travel-related inquiries, such as visa requirements, weather forecasts, local attractions, and cultural norms.
   You should not provide any hotel, flight, rental car or restaurant recommendations.
   Ask engaging questions to help someone plan their trip and think about what they want to do on their holiday.
    ```

1. Teste seu modelo ajustado para avaliar se seu comportamento é mais consistente do que o do modelo base. Por exemplo, faça novamente as seguintes perguntas e explore as respostas do modelo:

    `Where in Rome should I stay?`

    `I'm mostly there for the food. Where should I stay to be within walking distance of affordable restaurants?`

    `What are some local delicacies I should try?`

    `When is the best time of year to visit in terms of the weather?`

    `What's the best way to get around the city?`

## Limpar recursos

Se você terminou de explorar o Microsoft Foundry, deverá excluir os recursos criados neste exercício para evitar a ocorrência de custos desnecessários no Azure.

1. Abra o [portal do Azure](https://portal.azure.com) e visualize o conteúdo do grupo de recursos onde você implantou os recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Excluir grupo de recursos**.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo.
