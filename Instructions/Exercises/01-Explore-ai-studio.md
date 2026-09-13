---
lab:
  title: Preparar-se para um projeto de desenvolvimento de IA
  description: Aprenda a organizar recursos de IA em um projeto do Microsoft Foundry e comece a usar a extensão Foundry Toolkit para Visual Studio Code.
  level: 200
  duration: 30
  islab: true
  status: 'released'
  primarytopics:
    - Microsoft Foundry
    - Visual Studio Code
---

# Preparar-se para um projeto de desenvolvimento de IA

Neste exercício, você usará o portal do Microsoft Foundry para criar um projeto pronto para desenvolver uma solução de IA.

Este exercício leva aproximadamente **30** minutos.

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

1. Se ainda não estiver habilitada, habilite a opção **New Foundry** na barra de ferramentas na parte superior da página. Em seguida, crie um novo projeto com um nome exclusivo; expanda a área **Advanced options** para especificar as seguintes configurações para o projeto:
    - **Foundry resource**: *Use o nome padrão do recurso (geralmente {project_name}-resource)*
    - **Subscription**: *Sua assinatura do Azure*
    - **Resource group**: *Crie ou selecione um grupo de recursos*
    - **Region**: selecione qualquer uma das regiões **AI Foundry recomendadas** nesta [lista](https://learn.microsoft.com/azure/foundry/openai/how-to/responses#region-availability){:target="_blank"}

    > **Dica**: anote a região selecionada. Você precisará dela mais tarde!

1. Selecione **Create**. Aguarde até que o projeto seja criado.

    Quando estiver pronto, a página inicial do projeto será aberta.

    ![Captura de tela da página inicial do projeto do Foundry.](../media/foundry-portal-home.png)

## Implantar e testar um modelo

No centro de qualquer projeto de IA generativa há pelo menos um modelo de IA generativa.

1. Agora você está pronto para explorar os modelos. Na página **Discover**, selecione a guia **Models** para exibir o catálogo de modelos do Microsoft Foundry.

1. Pesquise o modelo `gpt-5.2` e selecione-o nos resultados da pesquisa para exibir seu model card.

    Os model cards fornecem informações sobre os modelos para ajudar você a entender seus recursos e limitações e determinar se eles são adequados aos seus requisitos.

    ![Captura de tela do model card do gpt-5.2.](../media/gpt5.2-details.png)

1. Selecione **Deploy** com as configurações padrão para criar uma implantação do modelo.

    As implantações de modelos permitem trabalhar com um modelo no projeto.

    Quando o modelo for implantado, o model playground será aberto automaticamente para que você possa testá-lo:

    ![Captura de tela do model playground do projeto Foundry.](../media/ai-foundry-model-playground.png)

1. Na caixa **Instructions**, insira as seguintes instruções:

    ```text
    You are an AI assistant that can provide information and advice about AI software development.
    ```

1. Na janela de chat, insira uma consulta como `Describe three key considerations for working with Large Language Models for AI application development.` e examine a resposta:

    Esperamos que o modelo tenha fornecido algumas considerações importantes para você analisar!

## Exibir o recurso do Azure e os endpoints do projeto no Foundry

1. No portal do Foundry, selecione **Manage** na barra de menus superior.

    O centro de gerenciamento é onde você pode exibir e administrar seus projetos e os recursos pai deles.

    ![Captura de tela da guia Manage no portal do Foundry.](../media/ai-foundry-manage.png)

    - O nível de *resource* refere-se ao recurso do **Foundry** criado no Azure para dar suporte ao projeto. Esse recurso inclui conexões com o Foundry Services e modelos, além de fornecer um local central para gerenciar o acesso dos usuários aos projetos de desenvolvimento de IA.
    - O nível de *project* refere-se ao projeto individual, no qual você pode adicionar e gerenciar recursos específicos do projeto. Um recurso pode dar suporte a vários projetos (o primeiro criado é o projeto *default* do recurso).

1. Selecione o link do **Parent resource** associado ao projeto.

    Os detalhes de configuração do recurso devem ser exibidos.

    Observe que o recurso do Foundry tem um *endpoint* por meio do qual os aplicativos cliente podem acessar funcionalidades no nível do recurso, como as Foundry Tools compartilhadas entre todos os projetos do recurso.

1. Na barra de menus superior, selecione **Home** para retornar à página inicial do projeto.
1. Anote a chave, o endpoint do projeto e o endpoint do Azure OpenAI.

    Essas informações são usadas para conectar aplicativos cliente aos recursos no nível do projeto.

    - A *key* é usada para autenticação baseada em chave em modelos e ferramentas (embora, na maioria dos cenários de produção, você deva considerar o uso da autenticação do Microsoft Entra ID com base em identidades de usuários e aplicativos autenticados).
    - O *project endpoint* é usado para acessar modelos fornecidos diretamente no Foundry (incluindo modelos OpenAI) usando a API **Responses** da OpenAI e para acessar APIs específicas do Foundry, como o serviço Foundry Agent.
    - O *OpenAI endpoint* é usado para acessar modelos usando APIs da OpenAI, incluindo a API **Chat Completions** e a API **Responses**.

## Instalar a extensão Foundry Toolkit para Visual Studio Code

Como desenvolvedor, você pode passar algum tempo trabalhando no portal do Foundry, mas provavelmente também passará bastante tempo no Visual Studio Code. A extensão Foundry Toolkit oferece uma maneira conveniente de trabalhar com os recursos do projeto Foundry sem sair do ambiente de desenvolvimento.

1. Inicie o Visual Studio Code.
1. Na barra de navegação à esquerda, abra a página **Extensions**.
1. Pesquise `Foundry Toolkit` no marketplace de extensões e instale a extensão **Foundry Toolkit for VS Code**.

    A instalação da extensão pode levar cerca de um minuto.

1. Depois de instalar a extensão, selecione a página **Foundry Toolkit** na barra de navegação à esquerda e aguarde o carregamento.

    ![Captura de tela da extensão Foundry Toolkit para Visual Studio Code.](../media/foundry-vs-extension.png)

1. No painel Foundry Toolkit, expanda **Microsoft Foundry Resources** e defina o projeto padrão conectando-se ao Azure (entrando com suas credenciais) e selecionando o projeto Foundry criado anteriormente.

1. Depois de definir o projeto padrão, expanda o projeto, expanda **Models** e selecione o modelo **gpt-5.2** implantado anteriormente.

    Você pode exibir aqui os detalhes da implantação do modelo.

    ![Captura de tela de um modelo na extensão Foundry Toolkit para Visual Studio Code.](../media/vscode-extension-model.png)

1. No painel Foundry Toolkit, na seção **Developer Tools**, expanda **Build** e selecione **Model playground**. Em seguida, selecione o modelo **gpt-5.2** (se ele ainda não estiver selecionado).

    Um playground interativo no qual você pode testar o modelo será aberto no Visual Studio Code.

    ![Captura de tela do model playground no Visual Studio Code.](../media/vscode-model-playground.png)

## Resumo

Neste exercício, você criou um Microsoft Foundry e o explorou no portal do Foundry. Você também explorou a extensão Foundry Toolkit no Visual Studio Code, que oferece uma maneira conveniente para os desenvolvedores trabalharem com projetos do Foundry e seus ativos.

## Limpeza

Se você terminou de explorar o portal do Foundry, deverá excluir os recursos criados neste exercício para evitar custos desnecessários do Azure.

1. No [portal do Azure](https://portal.azure.com), em `https://portal.azure.com`, exiba o conteúdo do grupo de recursos no qual você implantou os recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Delete resource group**.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo.
