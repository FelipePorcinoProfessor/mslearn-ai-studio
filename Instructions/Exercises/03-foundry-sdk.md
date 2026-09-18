---
lab:
    title: 'Criar um aplicativo de chat de IA generativa'
    description: 'Aprenda a usar o OpenAI SDK e a Responses API para criar um aplicativo de chat que se conecta a um modelo implantado no Microsoft Foundry.'
    level: 300
    duration: 45
    islab: true
    status: 'released'
layout: default
---

# Criar um aplicativo de chat de IA generativa

Neste exercício, você usará o OpenAI SDK e a Responses API para criar um aplicativo de chat que se conecta a um modelo implantado em um projeto do Microsoft Foundry.

Este exercício leva aproximadamente **45** minutos.

> **Observação**: algumas das tecnologias usadas neste exercício estão em versão prévia ou em desenvolvimento ativo. Você pode encontrar comportamentos inesperados, avisos ou erros.

## Pré-requisitos

Antes de iniciar este exercício, certifique-se de que você tenha:

- Uma [assinatura ativa do Azure](https://azure.microsoft.com/pricing/purchase-options/azure-account)
- O [Visual Studio Code](https://code.visualstudio.com/) instalado
- A [versão **3.13.xx** do Python](https://www.python.org/downloads/release/python-31312/) instalada\*
- O [Git](https://git-scm.com/install/) instalado e configurado
- A [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalada

> \* O Python 3.14 está disponível, mas algumas dependências ainda não foram compiladas para essa versão. O laboratório foi testado com sucesso usando o Python 3.13.12.

## Criar um projeto do Microsoft Foundry

O Microsoft Foundry usa projetos para organizar modelos, recursos, dados e outros ativos usados no desenvolvimento de uma solução de IA.

1. Em um navegador da Web, abra o [portal do Microsoft Foundry](https://ai.azure.com) em `https://ai.azure.com` para começar a desenvolver; entre usando suas credenciais do Azure. Feche todas as dicas ou painéis de início rápido que forem abertos na primeira vez que você entrar.

1. Se ainda não estiver habilitada, habilite a opção **New Foundry** na barra de ferramentas na parte superior da página. Em seguida, se solicitado, crie um novo projeto com um nome exclusivo; expanda a área **Advanced options** para especificar as seguintes configurações para o projeto:
    - **Foundry resource**: *Use o nome padrão do recurso (geralmente {project_name}-resource)*
    - **Subscription**: *Sua assinatura do Azure*
    - **Resource group**: *Crie ou selecione um grupo de recursos*
    - **Region**: selecione qualquer uma das regiões **AI Foundry recomendadas** nesta [lista](https://learn.microsoft.com/azure/foundry/openai/how-to/responses#region-availability){:target="_blank"}

1. Aguarde até que o projeto seja criado. Em seguida, exiba sua página inicial.

## Implantar um modelo

Em seguida, vamos implantar um modelo que será usado no aplicativo de chat.

1. Agora você está pronto para explorar os modelos. Na página **Discover**, selecione a guia **Models** para exibir o catálogo de modelos do Microsoft Foundry.
1. No catálogo de modelos, pesquise `gpt-5.2`.
1. Examine o model card e implante o modelo usando as configurações padrão.
1. Quando o modelo for implantado, ele será aberto no model playground; se quiser, você poderá testá-lo nesse local.

## Obter o endpoint

Você precisará de um endpoint para conectar um aplicativo cliente ao modelo. Neste exercício, usaremos o OpenAI SDK para conversar com o modelo e usaremos o endpoint do Azure OpenAI com autenticação do Entra ID para nos conectarmos a ele.

> **Observação**: como alternativa à autenticação do Entra ID, você poderia usar a API Key do projeto. Sempre que possível, é preferível usar a autenticação do Entra ID.

1. Na barra de menus, selecione a página **Home**.
1. Anote o **Azure OpenAI Endpoint** exibido nessa página.

    > **Dica**: neste exercício, você usará o **Azure OpenAI Endpoint**, <u>não</u> o endpoint do projeto!

## Criar um aplicativo cliente para conversar com o modelo

Agora que você implantou um modelo, pode usar o OpenAI SDK e a Responses API para desenvolver um aplicativo de chat com ele.

### Obter os arquivos do aplicativo no GitHub

Os arquivos iniciais necessários para desenvolver o aplicativo de chat estão disponíveis em um repositório do GitHub.

1. Abra o Visual Studio Code.
1. Abra a paleta de comandos (*Ctrl+Shift+P*) e use o comando `Git:clone` para clonar o repositório `https://github.com/microsoftlearning/mslearn-ai-studio` em uma pasta local (pode ser qualquer uma). Em seguida, abra-o.

    Talvez seja solicitado que você confirme se confia nos autores.

### Preparar a configuração do aplicativo

1. No Visual Studio Code, abra o painel **Extensions** e, se ainda não estiver instalada, instale a extensão **Python**.
1. Na **Command Palette**, use o comando `python:select interpreter`. Em seguida, crie um ambiente **Venv** baseado na instalação do Python 3.13.

    > **Dica**: se for solicitado que você instale dependências, poderá instalar as que estão no arquivo *requirements.txt* da pasta */labfiles/foundry-chat/python/chat-app*; mas não há problema se você não fizer isso, pois nós as instalaremos mais tarde!

1. No painel Explorer, navegue até a pasta que contém os arquivos de código do aplicativo em **/labfiles/foundry-chat/python/chat-app**. Os arquivos do aplicativo incluem:
    - **.env** (o arquivo de configuração do aplicativo)
    - **requirements.txt** (as dependências de pacotes Python que precisam ser instaladas)
    - **chat-app.py** (o arquivo de código do aplicativo de chat)
    - **chat-async.py** (o arquivo de código de uma versão assíncrona do aplicativo)

1. No painel **Explorer**, clique com o botão direito do mouse na pasta **chat-app** que contém os arquivos do aplicativo e selecione **Open in integrated terminal** (ou abra um terminal no menu **Terminal** e navegue até a pasta */labfiles/foundry-chat/python/chat-app*.)

    > **Observação**: abrir o terminal no Visual Studio Code ativará automaticamente o ambiente Python. Talvez seja necessário habilitar a execução de scripts no sistema.

1. Certifique-se de que o terminal esteja aberto na pasta **labfiles/foundry-chat/python/chat-app**, com o prefixo **(.venv)** indicando que o ambiente Python criado está ativo.
1. Instale o OpenAI SDK, o Azure Identity e os outros pacotes necessários executando o seguinte comando:

    ```
    pip install -r requirements.txt
    ```

1. No painel **Explorer**, na pasta **labfiles/foundry-chat/python/chat-app**, selecione o arquivo **.env** para abri-lo. Em seguida, atualize os valores de configuração para incluir o **Azure OpenAI Endpoint** e o nome atribuído à implantação do modelo **gpt-5.2**.

    > **Dica**: copie o **Azure OpenAI Endpoint** (não o endpoint do projeto!) da página inicial do projeto no portal do Foundry e insira o nome exato da implantação atribuído à sua implantação na configuração `MODEL_DEPLOYMENT`.

    Salve o arquivo de configuração modificado.

### Usar a API *ChatCompletions* para conversar com o modelo

A API *ChatCompletions* é uma maneira consolidada de criar aplicativos cliente para large language models e foi amplamente adotada.

1. No painel **Explorer**, na pasta **labfiles/foundry-chat/python/chat-app**, selecione o arquivo **chat-app.py** (<u>não</u> o arquivo *chat-async.py*) para abri-lo.
1. Examine o código existente. Você adicionará código para usar o OpenAI SDK e acessar seu modelo.

    > **Dica**: ao adicionar código ao arquivo, mantenha a indentação correta.

1. Na parte superior do arquivo de código, sob as referências de namespace existentes, localize o comentário **Import namespaces** e adicione o código a seguir para importar o namespace necessário ao uso do OpenAI SDK:

    ```python
   # Importar namespaces
   from openai import OpenAI
   from azure.identity import DefaultAzureCredential, get_bearer_token_provider
    ```

1. Na função **main**, observe que o código para carregar o endpoint e a chave do arquivo de configuração já foi fornecido. Em seguida, localize o comentário **Initialize the OpenAI client** e adicione o código a seguir para criar um cliente para a API da OpenAI:

    ```python
   # Inicializar o cliente da OpenAI
   token_provider = get_bearer_token_provider(
        DefaultAzureCredential(), "https://ai.azure.com/.default"
   )
    
   openai_client = OpenAI(
        base_url=azure_openai_endpoint,
        api_key=token_provider
   )
    ```

1. Na função **main**, observe que foi fornecido código para solicitar um prompt do usuário até que ele encerre o aplicativo. Dentro desse loop, localize o comentário **Get a response** e adicione o seguinte código:

    ```python
   # Obter uma resposta
   completion = openai_client.chat.completions.create(
        model=model_deployment,
        messages=[
            {
                "role": "system",
                "content": "Você é um assistente de IA útil que responde a perguntas e fornece informações."
            },
            {
                "role": "user",
                "content": input_text
            }
        ]
   )
   print(completion.choices[0].message.content)
    ```

    Observe que a API *ChatCompletions* usa uma coleção JSON de *messages* para encapsular a conversa. Geralmente, elas consistem em um *system prompt* que fornece instruções ao modelo e um *user prompt* que inclui a entrada do usuário.

1. Salve as alterações no arquivo de código. Em seguida, no painel do terminal, use o comando a seguir para entrar no Azure.

    ```powershell
    az login
    ```

    > **Observação**: na maioria dos cenários, basta usar *az login*. No entanto, se você tiver assinaturas em vários tenants, poderá ser necessário especificar o tenant usando o parâmetro *--tenant*. Consulte [Sign into Azure interactively using the Azure CLI](https://learn.microsoft.com/cli/azure/authenticate-azure-cli-interactively) para obter detalhes.

1. Quando solicitado, siga as instruções para entrar no Azure. Em seguida, conclua o processo de entrada na linha de comando, examinando (e confirmando, se necessário) os detalhes da assinatura que contém o recurso do Foundry.
1. Depois de entrar, insira o comando a seguir para executar o aplicativo:

    ```powershell
    python chat-app.py
    ```

    O programa deverá ser executado no terminal (caso contrário, resolva os erros e tente novamente).

1. Quando solicitado, insira o seguinte prompt:

    ```input
    Fale-me sobre o chatbot ELIZA.
    ```

    Após alguns instantes, o aplicativo deverá responder com algumas informações sobre o chatbot ELIZA, criado na década de 1960.

1. Insira o prompt `quit` para encerrar o aplicativo.

### Usar a API *Responses* para conversar com o modelo

Embora a API *ChatCompletions* seja amplamente usada, ela está sendo cada vez mais substituída pela API *Responses*, mais recente. Vamos atualizar o código para usá-la.

1. No código de **chat-app.py**, na função **main**, substitua o código sob o comentário **Get a response** pelo código a seguir, que usa a API *Responses*.

    ```python
   # Obter uma resposta
   response = openai_client.responses.create(
                model=model_deployment,
                instructions="Você é um assistente de IA útil que responde a perguntas e fornece informações.",
                input=input_text
   )
   print(response.output_text)
    ```

    Observe a sintaxe mais simples, na qual a mensagem do sistema é atribuída ao parâmetro *instructions* e o prompt do usuário é atribuído ao parâmetro *input*.

1. Salve as alterações no código e, no painel do terminal, execute novamente o aplicativo (`python chat-app.py`).
1. Quando solicitado, insira o mesmo prompt de antes:

    ```input
    Fale-me sobre o chatbot ELIZA.
    ```

    Após alguns instantes, o aplicativo deverá responder novamente com algumas informações sobre o chatbot ELIZA.

1. Insira o prompt a seguir para tentar continuar a conversa:

    ```input
    Como ele se compara aos LLMs modernos?
    ```

    O aplicativo deverá responder de uma maneira que indique que não entende a que "ele" se refere. O contexto da conversa foi perdido. Vamos corrigir isso.

1. Insira o prompt `quit` para encerrar o aplicativo.

### Adicionar o controle da conversa

Para manter o contexto conversacional, precisamos incluir referências às respostas anteriores em cada nova solicitação.

1. No código de **chat-app.py**, na função **main**, localize o comentário **Loop until the user wants to quit** e adicione o código a seguir <u>acima</u> dele (*antes* do loop):

    ```python
   # Controlar respostas
   last_response_id = None
    ```

1. Modifique o código sob o comentário **Get a response** usando o código a seguir para transmitir o ID da resposta anterior na solicitação e, em seguida, obter o novo ID da resposta para que ele possa ser adicionado na próxima vez.

    ```python
   # Obter uma resposta
   response = openai_client.responses.create(
                model=model_deployment,
                instructions="Você é um assistente de IA útil que responde a perguntas e fornece informações.",
                input=input_text,
                previous_response_id=last_response_id,
   )
   print(response.output_text)
   last_response_id = response.id
    ```

    Usando essa técnica, você pode transmitir o ID da resposta anterior para manter o contexto. Também poderia implementar uma lógica mais complexa para transmitir um ID de qualquer resposta anterior, redirecionar uma conversa ou retomar uma thread conversacional anterior.

1. Salve as alterações no código e, no painel do terminal, execute novamente o aplicativo (`python chat-app.py`).
1. Quando solicitado, insira o mesmo prompt de antes:

    ```input
    Fale-me sobre o chatbot ELIZA.
    ```

    Após alguns instantes, o aplicativo deverá responder novamente com algumas informações sobre o chatbot ELIZA.

1. Insira o prompt a seguir para tentar continuar a conversa:

    ```input
    Como ele se compara aos LLMs modernos?
    ```

    Desta vez, o aplicativo deverá responder comparando o chatbot ELIZA com os LLMs modernos. A resposta pode ser bastante longa, e o aplicativo aguarda até receber tudo do modelo antes de exibi-la, o que pode fazer o aplicativo parecer sem resposta. Vamos corrigir isso a seguir!

1. Insira o prompt `quit` para encerrar o aplicativo.

### Implementar respostas em *streaming*

Para lidar com respostas longas, você pode usar *streaming* para começar a processar respostas parciais antes que o texto completo seja retornado.

1. No código de **chat-app.py**, na função **main**, substitua o código sob o comentário **Get a response** pelo código a seguir, que usa *streaming*.

    ```python
   # Obter uma resposta
   stream = openai_client.responses.create(
                model=model_deployment,
                instructions="Você é um assistente de IA útil que responde a perguntas e fornece informações.",
                input=input_text,
                previous_response_id=last_response_id,
                stream=True
   )
   for event in stream:
        if event.type == "response.output_text.delta":
            print(event.delta, end="")
        elif event.type == "response.completed":
            last_response_id = event.response.id
   print()
    ```

    Observe que o parâmetro *stream=True* cria uma resposta em streaming na qual *events* ocorrem à medida que cada novo bloco (ou *delta*) fica pronto para processamento.

1. Salve as alterações no código e, no painel do terminal, execute novamente o aplicativo (`python chat-app.py`).
1. Quando solicitado, insira o mesmo prompt de antes:

    ```input
    Fale-me sobre o chatbot ELIZA.
    ```

    Após alguns instantes, o aplicativo deverá começar a responder com algumas informações sobre o chatbot ELIZA. A resposta deverá aparecer de forma incremental à medida que cada bloco for retornado.

1. Insira o prompt a seguir para tentar continuar a conversa:

    ```input
    Como ele se compara aos LLMs modernos?
    ```

    Novamente, a resposta deverá ser exibida de forma incremental.

1. Insira o prompt `quit` para encerrar o aplicativo.

### Usar a API assíncrona

O OpenAI SDK oferece uma opção assíncrona que pode aumentar a capacidade de resposta dos aplicativos ao usar operações de longa duração com modelos ou agentes.

1. No painel **Explorer**, na pasta **labfiles/foundry-chat/python/chat-app**, selecione o arquivo **chat-async.py** (<u>não</u> o arquivo *chat-app.py*) para abri-lo.
1. Examine o código existente. Você adicionará código para usar a API assíncrona do OpenAI SDK e acessar seu modelo.

    > **Dica**: ao adicionar código ao arquivo, mantenha a indentação correta.

1. Na parte superior do arquivo de código, sob as referências de namespace existentes, localize o comentário **Import namespaces** e adicione o código a seguir para importar o namespace necessário ao uso do OpenAI SDK:

    ```python
   # Importar namespaces para execução assíncrona
   import asyncio
   from openai import AsyncOpenAI
   from azure.identity.aio import DefaultAzureCredential, get_bearer_token_provider
    ```

1. Na função **main**, observe que o código para carregar o endpoint e a chave do arquivo de configuração já foi fornecido. Em seguida, localize o comentário **Initialize an async OpenAI client** e adicione o código a seguir para criar um cliente para a API da OpenAI:

    ```python
   # Inicializar um cliente assíncrono da OpenAI
   credential = DefaultAzureCredential()
   token_provider = get_bearer_token_provider(
    credential, "https://ai.azure.com/.default"
   )

   async_client = AsyncOpenAI(
        base_url=azure_openai_endpoint,
        api_key=token_provider
   )
    ```

1. Na função **main**, observe que foi fornecido código para solicitar um prompt do usuário até que ele encerre o aplicativo. Dentro desse loop, localize o comentário **Await an asynchronous response** e adicione o seguinte código:

    ```python
   # Aguardar uma resposta assíncrona
   response = await async_client.responses.create(
                model=model_deployment,
                instructions="Você é um assistente de IA útil que responde a perguntas e fornece informações.",
                input=input_text,
                previous_response_id=last_response_id
   )
   assistant_text = response.output_text
   print("Assistant:", assistant_text)
   last_response_id = response.id
    ```

    Esse código aguarda uma resposta assíncrona do modelo.

1. No final da função **main**, no bloco **finally**, localize o comentário **Close the async client session** e adicione o seguinte código para fechar o cliente assíncrono:

    ```python
   # Fechar a sessão do cliente assíncrono
    await credential.close()
    ```

1. Salve as alterações no arquivo de código. Em seguida, no painel do terminal, use o seguinte comando para executar o programa:

    ```powershell
   python chat-async.py
    ```

    O programa deverá ser executado no terminal (caso contrário, resolva os erros e tente novamente).

1. Quando solicitado, insira o seguinte prompt:

    ```input
    Fale-me sobre o teste de Turing.
    ```

    Após alguns instantes, o aplicativo deverá responder com algumas informações sobre o teste de Turing.

1. Insira o prompt `quit` para encerrar o aplicativo.

## Resumo

Neste exercício, você usou o OpenAI SDK e as APIs *ChatCompletions* e *Responses* para criar um aplicativo cliente para um modelo de IA generativa implantado em um projeto do Microsoft Foundry. Você personalizou o comportamento do modelo controlando o contexto conversacional e implementou streaming para oferecer uma experiência de chat responsiva.

## Limpeza

Se você terminou de explorar o Microsoft Foundry, deverá excluir os recursos criados neste exercício para evitar custos desnecessários do Azure.

1. Abra o [portal do Azure](https://portal.azure.com) e exiba o conteúdo do grupo de recursos no qual você implantou os recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Delete resource group**.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo.
