---
lab:
  title: Criar um aplicativo de IA generativa que usa ferramentas
  description: Saiba como usar ferramentas para estender os recursos de um modelo.
  level: 300
  duration: 30
  islab: true
  status: 'released'
layout: default
---

# Criar um aplicativo de IA generativa que usa ferramentas

Neste exercício, você usará o portal Microsoft Foundry e a Responses API para criar um aplicativo de chat com IA. Em seguida, você integrará conhecimento ao seu aplicativo usando as ferramentas *web_search* e *file_search*.

Este exercício leva aproximadamente **30** minutos.

> **Observação**: Algumas das tecnologias usadas neste exercício estão em versão prévia ou em desenvolvimento ativo. Você pode experimentar comportamentos inesperados, avisos ou erros.

## Pré-requisitos

Antes de começar este exercício, verifique se você tem:

- Uma [assinatura do Azure](https://azure.microsoft.com/pricing/purchase-options/azure-account) ativa
- [Visual Studio Code](https://code.visualstudio.com/) instalado
- [Python versão **3.13.xx**](https://www.python.org/downloads/release/python-31312/) instalado\*
- [Git](https://git-scm.com/install/) instalado e configurado
- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalado

> \* O Python 3.14 está disponível, mas algumas dependências ainda não foram compiladas para essa versão. O laboratório foi testado com sucesso com o Python 3.13.12.

# Criar um projeto do Microsoft Foundry

O Microsoft Foundry usa projetos para organizar modelos, recursos, dados e outros ativos usados para desenvolver uma solução de IA.

1. Em um navegador da Web, abra o [portal Microsoft Foundry](https://ai.azure.com) em `https://ai.azure.com` para começar a criar; entrando com suas credenciais do Azure. Feche quaisquer dicas ou painéis de início rápido que sejam abertos na primeira vez que você entrar.

1. Se ainda não estiver habilitado, na barra de ferramentas na parte superior da página, ative a opção **New Foundry**. Em seguida, se solicitado, crie um novo projeto com um nome exclusivo; expandindo a área **Advanced options** para especificar as seguintes configurações para o seu projeto:
    - **Foundry resource**: *Use o nome padrão para seu recurso (geralmente {project_name}-resource)*
    - **Subscription**: *Sua assinatura do Azure*
    - **Resource group**: *Crie ou selecione um grupo de recursos*
    - **Region**: Selecione qualquer uma das regiões **AI Foundry recommended** nesta **[lista](https://learn.microsoft.com/azure/foundry/openai/how-to/responses#region-availability)**{:target="_blank"}

1. Aguarde a criação do seu projeto. Em seguida, exiba a página inicial do projeto.

## Implantar um modelo

Em seguida, vamos implantar um modelo que você usará em seu aplicativo de chat.

1. Agora você está pronto para explorar modelos. Na página **Discover**, selecione a guia **Models** para exibir o catálogo de modelos do Microsoft Foundry.
1. No catálogo de modelos, pesquise por `gpt-5.2`.
1. Revise a ficha do modelo e, em seguida, implante-o usando as configurações padrão.
1. Quando o modelo tiver sido implantado, ele será aberto no playground do modelo.

## Experimentar ferramentas no playground

Antes de desenvolver um aplicativo de chat, vamos explorar como o modelo responde no playground. Isso ajudará você a entender por que dados de grounding são importantes.

1. Depois de implantar seu modelo, você deve estar no playground com esse modelo selecionado. Caso contrário, selecione **Build** na barra de menu superior, depois selecione **Deployments** à esquerda e, em seguida, selecione o modelo que você implantou.
1. No playground do modelo, no painel à esquerda, verifique se o seu modelo **gpt-5.2** está selecionado.

1. No campo **Instructions**, insira o seguinte prompt:

    ```
   You are a travel assistant that provides information on travel services available from Margie's Travel.
    ```

1. No painel de chat, insira a consulta `What are some recommended tourist activities in New York next month?` e revise a resposta.

    A resposta deve ser bastante genérica — o modelo fornece conhecimento geral com base em seus dados de treinamento, mas não tem acesso a informações atuais sobre o que acontecerá em Nova York no próximo mês.

1. No painel à esquerda, abaixo das instruções, na seção **Tools**, selecione **Add** e adicione a ferramenta **web_search**.

1. No painel de chat, insira a mesma consulta `What are some recommended tourist activities in New York next month?` e revise a resposta.

    Desta vez, o modelo usa a ferramenta *web_search* para encontrar informações atuais sobre atividades em Nova York.

## Criar um aplicativo que usa ferramentas

Agora que você viu como as ferramentas podem estender os recursos de um modelo no playground, vamos criar um aplicativo de cliente que usa ferramentas para fornecer conselhos de viagem para clientes da Margie's Travel.

# Obter o endpoint

Você precisará de um endpoint para se conectar ao modelo a partir de um aplicativo cliente. Neste exercício, vamos usar o OpenAI SDK para conversar com o modelo; e usaremos o Azure OpenAI Endpoint com autenticação do Entra ID para nos conectar a ele.

> **Observação**: Como alternativa à autenticação do Entra ID, você pode usar a API Key do projeto. Usar a autenticação do Entra ID é preferível sempre que possível.

1. Na barra de menus, selecione a página **Home**.
1. Observe o **Azure OpenAI Endpoint** exibido ali.

    > **Dica**: Você usará o **Azure OpenAI Endpoint** neste exercício, <u>não</u> o endpoint do projeto!

### Obter os arquivos do aplicativo no GitHub

Os arquivos iniciais do aplicativo de que você precisará para desenvolver seu aplicativo de chat são fornecidos em um repositório do GitHub.

1. Abra o Visual Studio Code.
1. Abra a paleta de comandos (Ctrl+Shift+P) e use o comando `Git:clone` para clonar o repositório `https://github.com/microsoftlearning/mslearn-ai-studio` para uma pasta local (não importa qual). Em seguida, abra-o.

    Talvez você seja solicitado a confirmar que confia nos autores.

### Preparar a configuração do aplicativo

1. No Visual Studio Code, exiba o painel **Extensions**; e, se ainda não estiver instalada, instale a extensão **Python**.
1. Na **Command Palette**, use o comando `python:select interpreter`. Em seguida, selecione um ambiente existente, se você tiver um, ou crie um novo ambiente **Venv** com base na sua instalação do Python 3.1x.

    > **Dica**: Se você for solicitado a instalar dependências, poderá instalar as que estão no arquivo *requirements.txt* na pasta */labfiles/tools/python/tools-app*; mas tudo bem se não instalar agora — nós as instalaremos depois!

1. No painel Explorer, navegue até a pasta que contém os arquivos de código do aplicativo em **/labfiles/tools/python/tools-app**. Os arquivos do aplicativo incluem:
    - **brochures** (uma pasta que contém os folhetos da Margie's Travel)
    - **.env** (o arquivo de configuração do aplicativo)
    - **requirements.txt** (as dependências de pacotes Python que precisam ser instaladas)
    - **tools-app.py** (o arquivo de código do aplicativo)

1. No painel **Explorer**, clique com o botão direito do mouse na pasta **tools-app** que contém os arquivos do aplicativo e selecione **Open in integrated terminal** (ou abra um terminal no menu **Terminal** e navegue até a pasta */labfiles/tools/python/tools-app*.)

    > **Observação**: Abrir o terminal no Visual Studio Code ativará automaticamente o ambiente Python. Talvez seja necessário habilitar a execução de scripts no seu sistema.

1. Verifique se o terminal está aberto na pasta **/labfiles/tools/python/tools-app** com o prefixo **(.venv)** para indicar que o ambiente Python que você criou está ativo.
1. Instale o OpenAI SDK, Azure identity e outros pacotes necessários executando o seguinte comando:

    ```
    pip install -r requirements.txt
    ```

1. No painel **Explorer**, na pasta **/labfiles/tools/python/tools-app**, selecione o arquivo **.env** para abri-lo. Em seguida, atualize os valores de configuração para incluir o **Azure OpenAI Endpoint** e o nome atribuído à implantação do modelo **gpt-5.2**.

    > **Dica**: Copie o **Azure OpenAI Endpoint** (não o endpoint do projeto!) da página inicial do projeto no portal Foundry e insira o nome exato da implantação atribuída à sua implantação na configuração `MODEL_DEPLOYMENT`.

    Salve o arquivo de configuração modificado.

### Escrever código para implementar chat com ferramentas

1. No painel **Explorer**, na pasta **/labfiles/tools/python/tools-app**, selecione o arquivo **tools-app.py** para abri-lo.
1. Revise o código existente. Você adicionará código para usar o OpenAI SDK para acessar seu modelo.

    > **Dica**: Ao adicionar código ao arquivo, certifique-se de manter a indentação correta.

1. No início do arquivo de código, sob as referências de namespace existentes, localize o comentário **Import namespaces** e adicione o seguinte código para importar o namespace necessário para usar o OpenAI SDK:

    ```python
   # import namespaces
   from openai import OpenAI
   from azure.identity import DefaultAzureCredential, get_bearer_token_provider
    ```

1. Na função **main**, observe que já foi fornecido código para carregar o endpoint e a chave do arquivo de configuração. Em seguida, localize o comentário **Initialize the OpenAI client** e adicione o seguinte código para criar um cliente para a OpenAI API:

    ```python
   # Initialize the OpenAI client
   token_provider = get_bearer_token_provider(
        DefaultAzureCredential(), "https://ai.azure.com/.default"
   )
    
   openai_client = OpenAI(
        base_url=azure_openai_endpoint,
        api_key=token_provider
   )
    ```

1. Na função **main**, localize o comentário **Create vector store and upload files** e adicione o seguinte código:

    ```python
   # Create vector store and upload files
   print("Creating vector store and uploading files...")
   vector_store = openai_client.vector_stores.create(
        name="travel-brochures"
   )
   file_streams = [open(f, "rb") for f in glob.glob("brochures/*.pdf")]
   if not file_streams:
        print("No PDF files found in the brochures folder!")
        return
   file_batch = openai_client.vector_stores.file_batches.upload_and_poll(
        vector_store_id=vector_store.id,
        files=file_streams
   )
   for f in file_streams:
        f.close()
   print(f"Vector store created with {file_batch.file_counts.completed} files.")
    ```

    Esse código cria um vector store para seu modelo e carrega os folhetos nele. Usaremos esse vector store com a ferramenta *file_search*.

1. Na função **main**, observe que já foi fornecido código para solicitar um prompt do usuário até o usuário sair do aplicativo. Dentro desse loop, localize o comentário **Get a response using tools** e adicione o seguinte código:

    ```python
   # Get a response using tools
   response = openai_client.responses.create(
        model=model_deployment,
        instructions="""
        You are a travel assistant that provides information on travel services available from Margie's Travel.
        Answer questions about services offered by Margie's Travel using the provided travel brochures.
        Search the web for general information about destinations or current travel advice.
        """,
        input=input_text,
        previous_response_id=last_response_id,
        tools=[
            {
                "type": "file_search",
                "vector_store_ids": [vector_store.id]
            },
            {
                "type": "web_search"
            }
        ]
   )
   print(response.output_text)
   last_response_id = response.id
    ```

    Esse código envia um prompt e especifica que a ferramenta *file_search* pode ser usada para pesquisar o vector store e a ferramenta *web_search* pode ser usada para pesquisas gerais na Web.

1. Salve as alterações no arquivo de código. Em seguida, no painel do terminal, use o seguinte comando para entrar no Azure.

    ```powershell
    az login
    ```

    > **Observação**: Na maioria dos cenários, usar apenas *az login* será suficiente. No entanto, se você tiver assinaturas em vários tenants, talvez seja necessário especificar o tenant usando o parâmetro *--tenant*. Consulte [Entrar no Azure de forma interativa usando a Azure CLI](https://learn.microsoft.com/cli/azure/authenticate-azure-cli-interactively) para obter detalhes.

1. Quando solicitado, siga as instruções para entrar no Azure. Em seguida, conclua o processo de entrada na linha de comando, visualizando (e confirmando, se necessário) os detalhes da assinatura que contém seu recurso Foundry.
1. Depois de entrar, insira o seguinte comando para executar o aplicativo:

    ```powershell
   python tools-app.py
    ```

    O programa deve ser executado no terminal (se não, resolva quaisquer erros e tente novamente).

1. Quando solicitado, insira `What's happening in San Francisco next month?` e revise a resposta do seu modelo de IA generativa.

    A resposta deve incluir informações recuperadas usando a ferramenta *web_search*.

1. Experimente esta pergunta de acompanhamento: `What hotels does Margie's Travel offer there?`

    A resposta deve incluir informações recuperadas usando a ferramenta *file_search*.

1. Quando terminar, insira `quit` para sair do programa.

## Limpar

Se você terminou de explorar o Microsoft Foundry, deve excluir os recursos criados neste exercício para evitar custos desnecessários do Azure.

1. Abra o [portal do Azure](https://portal.azure.com) e exiba o conteúdo do grupo de recursos onde você implantou os recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Delete resource group**.
1. Insira o nome do grupo de recursos e confirme que você deseja excluí-lo.
