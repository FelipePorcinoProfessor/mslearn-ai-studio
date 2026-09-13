---
lab:
  title: Realizar fine-tune de um modelo de linguagem
  description: Aprenda como utilizar seus próprios dados de treinamento para realizar fine-tune de um modelo e personalizar seu comportamento.
  level: 300
  duration: 90
  islab: true
  status: 'released'
---

# Realizar fine-tune de um modelo de linguagem

Quando você deseja que um modelo de linguagem se comporte de uma determinada maneira, pode utilizar prompt engineering para definir o comportamento desejado. Quando quiser melhorar a consistência desse comportamento, você pode optar por realizar fine-tune do modelo, comparando-o com sua abordagem de prompt engineering para avaliar qual método atende melhor às suas necessidades.

Neste exercício, você realizará o fine-tune de um modelo de linguagem utilizando o Microsoft Foundry para um cenário de aplicação de chat personalizada. Você comparará o modelo com fine-tune a um base model para avaliar se o modelo com fine-tune atende melhor às suas necessidades.

Imagine que você trabalha em uma agência de viagens e está desenvolvendo uma aplicação de chat para ajudar pessoas a planejarem suas férias. O objetivo é criar um chat simples e inspirador que sugira destinos e atividades utilizando um tom de conversa consistente e amigável.

Este exercício levará aproximadamente **90** minutos\*.

> \* **Observação**: Esse tempo é uma estimativa baseada na experiência média. O processo de fine-tuning depende dos recursos da infraestrutura de cloud, que podem levar tempos variáveis para serem provisionados dependendo da capacidade do data center e da demanda simultânea. Algumas atividades deste exercício podem levar um tempo <u>considerável</u> para serem concluídas e exigem paciência. Caso o processo esteja demorando, considere consultar a [documentação de fine-tuning do Microsoft Foundry](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/fine-tuning?view=foundry) ou fazer uma pausa. É possível que alguns processos apresentem time-out ou pareçam ser executados indefinidamente. Algumas das tecnologias utilizadas neste exercício estão em preview ou em desenvolvimento ativo. Você pode encontrar comportamentos inesperados, warnings ou errors.

## Pré-requisitos

Para concluir este exercício, você precisa de:

- Uma [Azure subscription](https://azure.microsoft.com/free/) com permissões para criar recursos de AI.

## Criar um projeto no Microsoft Foundry

O Microsoft Foundry utiliza projects para organizar models, resources, data e outros assets utilizados no desenvolvimento de uma solução de AI.

1. Em um web browser, abra o [Microsoft Foundry portal](https://ai.azure.com) em `https://ai.azure.com` para começar; faça login utilizando suas credenciais do Azure. Feche quaisquer painéis de dicas ou quick start que forem abertos na primeira vez que você fizer login.

1. Caso ainda não esteja habilitada, na tool bar na parte superior da página, habilite a opção **New Foundry**. Em seguida, caso solicitado, crie um novo project com um nome exclusivo; expanda a área **Advanced options** para especificar as seguintes configurações para seu project:
    - **Foundry resource**: *Utilize o nome padrão para seu resource (normalmente {project_name}-resource)*
    - **Subscription**: *Sua Azure subscription*
    - **Resource group**: *Crie ou selecione um resource group*
    - **Region**: *Selecione uma das seguintes regions*:\*
        - North Central US
        - Sweden Central

    > \* No momento da elaboração deste material, essas regions oferecem suporte ao fine-tuning de modelos gpt-5. Consulte a [página de models](https://learn.microsoft.com/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure?&pivots=azure-openai#fine-tuning-models) para verificar a disponibilidade mais recente por region.

1. Aguarde até que seu project seja criado. Em seguida, acesse sua home page.

## Fazer deploy de um model

Agora, vamos fazer deploy de um model que será utilizado para obter uma baseline de performance.

1. Agora você está pronto para explorar os models. Na página **Discover**, selecione a guia **Models** para visualizar o model catalog do Microsoft Foundry.
1. No model catalog, pesquise por `gpt-5`.
1. Analise o model card e, em seguida, faça o deploy utilizando as configurações padrão.
1. Quando o model tiver sido deployed, ele será aberto no model playground.

## Realizar fine-tune de um model

Como o processo de fine-tuning de um model leva algum tempo para ser concluído, você iniciará agora o fine-tuning job e retornará a ele depois de explorar o base model gpt-5 que já foi deployed.

1. Faça download do [training dataset](https://microsoftlearning.github.io/mslearn-ai-studio/data/travel-finetune-hotel.jsonl) em `https://microsoftlearning.github.io/mslearn-ai-studio/data/travel-finetune-hotel.jsonl` e salve-o localmente como um arquivo JSONL.

    > **Observação**: Seu dispositivo pode salvar o arquivo como `.txt` por padrão. Selecione a opção para exibir todos os tipos de arquivos e remova o sufixo `.txt` para garantir que o arquivo seja salvo como JSONL.

1. No Foundry portal, enquanto visualiza o model playground, no painel de navegação à esquerda, selecione **Fine-tune**.
1. Selecione o botão **Fine-tune** no canto superior direito e configure o fine-tuning job com as seguintes opções:
    - **Base model**: Selecione **gpt-5**
    - **Customization method**: Supervised
    - **Training type**: Standard
    - **Training data**: Selecione **Upload new dataset** e faça upload do arquivo `.jsonl` que você baixou anteriormente.
    - **Suffix**: `ft-travel`
    - **Automatically deploy model after job completion**: Selecionado
    - **Deployment type**: Developer
    - *Mantenha os demais hyperparameters com seus valores padrão*
1. Selecione **Submit** para iniciar o fine-tuning job. O processo pode levar algum tempo para ser concluído. Enquanto aguarda, você pode continuar com a próxima seção do exercício.

> **Observação**: O fine-tuning e o deployment podem levar um tempo significativo, cerca de 60 minutos ou mais, portanto talvez seja necessário verificar o progresso periodicamente. Você pode visualizar mais detalhes do progresso selecionando o fine-tuning job e acessando a guia **Monitor**.

## Conversar com um base model

Enquanto aguarda a conclusão do fine-tuning job, vamos conversar com um foundation model *gpt-5* para avaliar seu comportamento.

1. No painel à esquerda, selecione **Deployments** e, em seguida, selecione o base model **gpt-5** que você fez deploy anteriormente.
1. No painel de chat, insira o prompt `O que você pode fazer?` e observe a response.

    As respostas podem ser bastante genéricas. Lembre-se de que queremos criar uma aplicação de chat que inspire as pessoas a viajar.

1. Altere as **Instructions** do model para o seguinte prompt:

    ```
    Você é um assistente de AI que ajuda as pessoas a planejarem suas viagens.
    ```

1. Na janela de chat, insira novamente a pergunta `O que você pode fazer?` e observe a response.

    Como response, o assistente pode informar que pode ajudá-lo a reservar voos, hotéis e carros para sua viagem. Queremos evitar esse comportamento.

1. No campo **Instructions**, insira um novo prompt:

    ```
    Você é um assistente de AI para viagens que ajuda as pessoas a planejarem suas viagens. Seu objetivo é oferecer suporte para dúvidas relacionadas a viagens, como requisitos de visto, previsão do tempo, atrações locais e costumes culturais.
    Você não deve fornecer recomendações de hotéis, voos, aluguel de carros ou restaurantes.
    Faça perguntas envolventes para ajudar a pessoa a planejar sua viagem e refletir sobre o que deseja fazer durante suas férias.
    ```

1. Continue testando o model para analisar seu comportamento. Por exemplo, faça as perguntas a seguir e observe as respostas do model, prestando atenção especial ao tom e ao estilo de escrita utilizados pelo model:

    `Em qual região de Roma eu deveria me hospedar?`

    `Estou indo principalmente pela comida. Onde devo me hospedar para ficar a uma distância que possa ser percorrida a pé de restaurantes com preços acessíveis?`

    `Quais são algumas iguarias locais que eu deveria experimentar?`

    `Qual é a melhor época do ano para visitar considerando o clima?`

    `Qual é a melhor maneira de se locomover pela cidade?`

## Analisar o arquivo de treinamento

O base model parece funcionar razoavelmente bem, mas talvez você esteja procurando um estilo de conversa específico para sua aplicação de generative AI. Os training data utilizados no fine-tuning permitem criar exemplos explícitos dos tipos de response que você deseja.

1. Abra o arquivo JSONL que você baixou anteriormente. Você pode abri-lo em qualquer text editor.
1. Examine a lista de documentos JSON no arquivo de training data. O primeiro deve ser semelhante ao exemplo abaixo, formatado para facilitar a leitura:

    ```json
    {"messages": [
        {"role": "system", "content": "Você é um assistente de AI para viagens que ajuda as pessoas a planejarem suas viagens. Seu objetivo é oferecer suporte para dúvidas relacionadas a viagens, como requisitos de visto, previsão do tempo, atrações locais e costumes culturais. Você não deve fornecer recomendações de hotéis, voos, aluguel de carros ou restaurantes. Faça perguntas envolventes para ajudar a pessoa a planejar sua viagem e refletir sobre o que deseja fazer durante suas férias."},
        {"role": "user", "content": "Qual é uma atração imperdível em Paris?"},
        {"role": "assistant", "content": "Oh là là! Você simplesmente precisa dar uma volta pela Torre Eiffel e tirar uma selfie cheia de estilo! Depois disso, considere visitar o Museu do Louvre para conhecer a Mona Lisa e outras obras-primas. Em quais tipos de atrações você tem mais interesse?"}
        ]}
    ```

    Cada exemplo de interação na lista inclui a mesma system message que você testou com o base model, um user prompt relacionado a uma pergunta sobre viagens e uma response. O estilo das responses presentes nos training data ajudará o modelo com fine-tune a aprender como ele deve responder.

## Testar o model com fine-tune

Quando seu model com fine-tune estiver pronto, você poderá testá-lo da mesma forma que testou o base model deployed.

1. No painel à esquerda, selecione **Fine-tune** e analise o status do fine-tuning job iniciado anteriormente.
1. Selecione o job para visualizar seus detalhes. Você pode utilizar a guia **Logs** para analisar as tarefas de fine-tuning que já foram executadas.
1. Quando o fine-tuning for concluído e o model tiver sido automaticamente deployed, acesse a página **Deployments** para verificar se ele está listado.

    > **Dica**: Caso o deployment automático falhe, selecione o fine-tuning job concluído e faça o deploy do model a partir dele.

1. Selecione o model com fine-tune para abri-lo no model playground.
1. Atualize as **Instructions** para utilizar o mesmo conteúdo testado com o base model:

    ```
    Você é um assistente de AI para viagens que ajuda as pessoas a planejarem suas viagens. Seu objetivo é oferecer suporte para dúvidas relacionadas a viagens, como requisitos de visto, previsão do tempo, atrações locais e costumes culturais.
    Você não deve fornecer recomendações de hotéis, voos, aluguel de carros ou restaurantes.
    Faça perguntas envolventes para ajudar a pessoa a planejar sua viagem e refletir sobre o que deseja fazer durante suas férias.
    ```

1. Teste seu model com fine-tune para avaliar se seu comportamento é mais consistente do que o base model. Por exemplo, faça novamente as perguntas abaixo e analise as respostas do model:

    `Em qual região de Roma eu deveria me hospedar?`

    `Estou indo principalmente pela comida. Onde devo me hospedar para ficar a uma distância que possa ser percorrida a pé de restaurantes com preços acessíveis?`

    `Quais são algumas iguarias locais que eu deveria experimentar?`

    `Qual é a melhor época do ano para visitar considerando o clima?`

    `Qual é a melhor maneira de se locomover pela cidade?`

## Limpeza

Se você terminou de explorar o Microsoft Foundry, deve excluir os resources criados neste exercício para evitar custos desnecessários no Azure.

1. Abra o [Azure portal](https://portal.azure.com) e visualize o conteúdo do resource group no qual você fez deploy dos resources utilizados neste exercício.
1. Na toolbar, selecione **Delete resource group**.
1. Insira o nome do resource group e confirme que deseja excluí-lo.
