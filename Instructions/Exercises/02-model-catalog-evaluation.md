---
lab:
  title: Explorar e comparar modelos
  description: Explore o catálogo de modelos para encontrar e comparar modelos e avalie o desempenho do modelo.
  level: 300
  duration: 45
  islab: true
  status: 'released'
layout: default
---

# Explorar e comparar modelos

O catálogo de modelos do Microsoft Foundry funciona como um repositório central onde você pode explorar e usar uma variedade de modelos, facilitando a criação do seu cenário de IA generativa. Neste exercício, você explorará o catálogo de modelos, comparará modelos usando benchmarks, testará modelos no model playground e executará uma avaliação usando um conjunto de dados sintético.

Este exercício levará aproximadamente **45** minutos.

> **Observação**: Algumas das tecnologias usadas neste exercício estão em versão prévia ou em desenvolvimento ativo. Você pode encontrar comportamentos inesperados, avisos ou erros.

## Pré-requisitos

Para concluir este exercício, você precisa de:

- Uma [Azure subscription](https://azure.microsoft.com/free/) com permissões para criar recursos de IA.

## Criar um projeto do Microsoft Foundry

O Microsoft Foundry usa projetos para organizar modelos, recursos, dados e outros ativos usados no desenvolvimento de uma solução de IA.

1. Em um navegador da Web, abra o [portal do Microsoft Foundry](https://ai.azure.com) em `https://ai.azure.com` para começar a criar; faça logon usando suas credenciais do Azure. Feche quaisquer dicas ou painéis de início rápido que forem abertos na primeira vez que você entrar.

1. Se ainda não estiver habilitada, na barra de ferramentas na parte superior da página, habilite a opção **New Foundry**. Em seguida, se for solicitado, crie um novo projeto com um nome exclusivo; expanda a área **Advanced options** para especificar as seguintes configurações para o seu projeto:
    - **Foundry resource**: *Use o nome padrão para o seu recurso (geralmente {project_name}-resource)*
    - **Subscription**: *Sua Azure subscription*
    - **Resource group**: *Crie ou selecione um resource group*
    - **Region**: Selecione qualquer uma das regiões **AI Foundry recommended** nesta **[esta lista](https://learn.microsoft.com/azure/foundry/openai/how-to/responses#region-availability)**{:target="_blank"}

1. Aguarde a criação do seu projeto. Em seguida, visualize sua página inicial.

## Explorar modelos no catálogo

O Microsoft Foundry Models fornece um catálogo de modelos que você pode usar no seu projeto. Você pode navegar pelo catálogo e comparar modelos para encontrar o mais adequado às suas necessidades.

1. Agora você está pronto para explorar modelos. Na página **Discover**, selecione a guia **Models** para ver o catálogo de modelos do Microsoft Foundry.

    O catálogo de modelos lista todos os modelos disponíveis no Foundry. Alguns são fornecidos diretamente pela Azure (e cobrados por meio da sua Azure subscription), enquanto outros são fornecidos por parceiros e pela comunidade.

    Observe que você pode pesquisar e filtrar o catálogo com base em nomes de modelos, recursos e outros fatores.

1. Pesquise por `gpt-5.2`. Em seguida, nos resultados da pesquisa, selecione o modelo **gpt-5.2** para ver seu *cartão do modelo*. Os cartões do modelo fornecem informações sobre os modelos para ajudá-lo a determinar se eles são adequados às suas necessidades.
1. Leia a descrição e revise as outras informações disponíveis na página **Details**.
1. Veja a página **Benchmarks** do modelo gpt-5.2 para ver como o modelo se compara em alguns benchmarks de desempenho padrão com outros modelos usados em cenários semelhantes.
1. Use a seta para voltar (**&larr;**) ao lado do título da página **gpt-5.2** para retornar ao catálogo de modelos.

## Comparar modelos usando o model leaderboard

Agora vamos usar os recursos de model leaderboard e de comparação lado a lado para comparar modelos visualmente.

1. Na página do catálogo de modelos, selecione **View leaderboard**.
1. Na página **Model leaderboard**, revise os principais modelos classificados por qualidade, segurança, custo e desempenho. Observe quais modelos obtêm as pontuações mais altas para métricas de qualidade de IA.
1. Role para baixo para usar a seção **Trade-off chart** para comparar modelos em várias dimensões.
1. Selecione **Benchmark Cost** no menu suspenso para ver como a qualidade do modelo se relaciona com o custo e, em seguida, use a lista de modelos para comparar **gpt-5.2** e **gpt-5-mini**. Se quiser explorar mais, você pode adicionar outros modelos à comparação.
1. Selecione a métrica **Throughput** no menu suspenso para ver como a qualidade desses modelos se relaciona com as pontuações de throughput.
1. Selecione a métrica **Safety** no menu suspenso para ver como a qualidade desses modelos se relaciona com as pontuações de segurança.
1. Na tabela logo acima dos gráficos de trade-off, você pode comparar benchmarks. Selecione **gpt-5.2** e **gpt-5-mini** e, opcionalmente, quaisquer outros modelos que você queira explorar e, em seguida, use o botão **Compare models** para ver seus benchmarks lado a lado.
1. Revise a comparação nos seguintes dados:
    - **Performance benchmarks**: pontuações de qualidade, segurança e throughput.
    - **Input** e **output**: os formatos compatíveis para prompts e respostas.
    - **Context**: o número de tokens que podem ser mantidos em uma conversa e produzidos como saída e quando o modelo foi treinado.
    - **Endpoints**: os endpoints de API pelos quais o modelo pode ser consumido por aplicativos cliente e se ele pode ser usado por um agente.
    - **Supported features**: recursos específicos que você pode exigir no cenário do seu aplicativo.
1. Use a seta para voltar (**&larr;**) ao lado do título da página **gpt-5.2** para retornar ao catálogo de modelos.

## Implantar modelos

Agora vamos implantar os modelos que usaremos para teste e avaliação. Você precisa implantar **gpt-5.2** e **gpt-5-mini**.

### Implantar o modelo gpt-5.2

1. No catálogo de modelos, pesquise por `gpt-5.2` e selecione-o.
1. Na página do modelo, selecione **Deploy** e implante o modelo usando as configurações padrão.

    O modelo implantado será aberto no model playground, onde ele será selecionado na lista suspensa **Model**.

1. Anote o nome de implantação atribuído ao modelo **gpt-5.2**. Você precisará identificar essa implantação mais tarde.

### Implantar o modelo gpt-5-mini

1. No model playground, na lista **Model**, selecione **Browse more models**.
1. Pesquise por `gpt-5-mini` e, em seguida, selecione-o e implante-o.

    O modelo é implantado e selecionado no model playground.

1. Anote o nome de implantação atribuído ao modelo **gpt-5-mini**.

## Comparar modelos no model playground

Agora que você tem duas implantações de modelo, vamos compará-las no playground.

1. No playground, verifique se a implantação do modelo **gpt-5-mini** está selecionada na lista **Models** e, em seguida, no lado direito da página, na lista **Compare models**, selecione a implantação do modelo **gpt-5.2**.
1. A visualização de comparação lado a lado é aberta diretamente em painéis de chat separados para cada modelo. Selecione a guia **Chat** para ambos os modelos e insira o seguinte prompt:

    ```
   I have a fox, a chicken, and a bag of grain that I need to take over a river in a boat. I can only take one thing at a time. If I leave the chicken and the grain unattended, the chicken will eat the grain. If I leave the fox and the chicken unattended, the fox will eat the chicken. How can I get all three things across the river without anything being eaten?
    ```

1. Envie o prompt e veja as respostas de ambos os modelos. Em seguida, insira o seguinte prompt de acompanhamento:

    ```
   Explain your reasoning.
    ```

1. Compare as respostas de cada modelo. Observe quaisquer diferenças em precisão, qualidade de raciocínio e estilo de resposta.

## Avaliar um modelo com um conjunto de dados sintético

O model playground é útil para testes manuais rápidos, mas, para avaliar sistematicamente o desempenho de um modelo em várias entradas, você pode executar uma avaliação. Vamos avaliar o modelo **gpt-5.2** usando um conjunto de dados gerado sinteticamente de perguntas relacionadas a viagens.

### Etapa 1: Target

1. No playground, selecione a guia **Evaluations**.
1. Selecione **Create** para abrir o assistente **Create new evaluation**.
1. Para o destino da avaliação, selecione **Model**.
1. Na tabela de modelos, desmarque quaisquer implantações pré-selecionadas para que apenas a caixa de seleção de **gpt-5.2** esteja selecionada e, em seguida, selecione **Next**.

### Etapa 2: Data

Em vez de enviar um conjunto de dados de teste, você usará o recurso de geração de dados sintéticos do Foundry para criar um automaticamente.

1. Na etapa **Data**, em **Dataset source**, selecione **Synthetic generation**.

    Com a geração sintética, uma implantação é usada para gerar automaticamente perguntas para cada destino quando você envia a avaliação.

1. Selecione **Generate** e, em seguida, defina e confirme o seguinte:
    - **Name of the new dataset**: *Deixe como padrão*
    - **Model**: gpt-5.2
    - **Number of rows**: 45
    - **Prompt**: `Create various travel related questions, and include some content safety and security tests`
    - **Seed data**: *Deixe em branco*
1. Selecione **Next** para continuar.

### Etapa 3: Configure models

1. Na etapa **Configure models**, defina o prompt de **Developer** para o modelo que está sendo avaliado:

    ```
    You are a helpful travel assistant that provides accurate, detailed, and practical travel advice to help users plan their trips.
    ```

1. Deixe o restante dos valores como padrão e selecione **Next**.

### Etapa 4: Criteria

1. Na etapa **Criteria**, veja todos os avaliadores sugeridos. Eles usam um modelo de IA como juiz para avaliar a qualidade das respostas.
1. Remova todos os critérios em *Agents* e *Safety*, deixando o restante dos avaliadores habilitados.
1. Selecione **Next**.

### Etapa 5: Review and submit

1. Na etapa **Review**, verifique a configuração da avaliação, incluindo o modelo de destino, o conjunto de dados e os critérios selecionados.
1. Forneça um nome para a avaliação, como `travel-assistant-eval`.
1. Selecione **Submit** para iniciar a execução da avaliação.
1. Aguarde a conclusão da avaliação. Isso pode levar alguns minutos, dependendo da carga do data center.

### Revisar os resultados

1. Quando a avaliação for concluída, selecione a execução da avaliação para ver a página de resultados, que exibe uma visão geral das métricas de avaliação.
1. Reveja as pontuações e os resultados de cada avaliação na tabela detalhada na página da execução. Role para a direita e veja páginas adicionais, onde você verá, em sua maioria, valores aprovados. Dependendo da resposta do modelo, você poderá ver algumas reprovações. Se vir, examine-as atentamente.
1. Selecione o botão **Analyze results**, selecionando **gpt-5.2** no menu suspenso e, em seguida, selecione **Start analysis**.
1. Nesta página, você verá quaisquer falhas agrupadas pelo motivo da falha, onde é possível ver detalhes sobre por que falhou. A maioria dessas falhas será devido ao modelo dizer que não pode ajudar devido à natureza da pergunta; no entanto, você deve explorar cada falha e considerar se a resposta é a que você deseja ver.
1. Revise quaisquer falhas e as sugestões de IA sobre como melhorar. Essas orientações ajudarão você a ajustar sua configuração para obter um desempenho melhor.

## Limpar

Se você terminou de explorar o Microsoft Foundry, deve excluir os recursos criados neste exercício para evitar custos desnecessários na Azure.

1. Abra o [Azure portal](https://portal.azure.com) e veja o conteúdo do resource group onde você implantou os recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Delete resource group**.
1. Insira o nome do resource group e confirme que deseja excluí-lo.
