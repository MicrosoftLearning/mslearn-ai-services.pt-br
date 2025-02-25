---
lab:
  title: Introdução aos Serviços de IA do Azure
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Introdução aos Serviços de IA do Azure

Neste exercício, você começará a usar os Serviços de IA do Azure criando um recurso dos **Serviços de IA do Azure** em sua assinatura do Azure e usando-o de um aplicativo cliente. O objetivo do exercício não é obter experiência em nenhum serviço específico, mas sim se familiarizar com um padrão geral para provisionar e trabalhar com os serviços de IA do Azure como desenvolvedor.

## Clonar o repositório no Visual Studio Code

Você desenvolverá seu código usando o Visual Studio Code. Os arquivos de código para seu aplicativo foram fornecidos em um repositório do GitHub.

> **Dica**: Se você já clonou o repositório **mslearn-ai-services**, abra-o no Visual Studio Code. Caso contrário, siga estas etapas para cloná-lo em seu ambiente de desenvolvimento.

1. Inicie o Visual Studio Code.
2. Abra a paleta (SHIFT+CTRL+P) e execute o comando **Git: Clone** para clonar o repositório `https://github.com/MicrosoftLearning/mslearn-ai-services` em uma pasta local (não importa qual pasta).
3. Depois que o repositório for clonado, abra a pasta no Visual Studio Code.
4. Aguarde enquanto arquivos adicionais são instalados para dar suporte aos projetos de código C# no repositório, se necessário

    > **Observação**: se você for solicitado a adicionar ativos necessários para compilar e depurar, selecione **Agora não**.

5. Expanda a pasta `Labfiles/01-use-azure-ai-services`.

Os códigos tanto em C# quanto em Python foram fornecidos. Expanda a pasta da linguagem escolhida.

## Provisionar um recurso dos Serviços de IA do Azure

Os Serviços de IA do Azure são serviços baseados em nuvem que encapsulam recursos de inteligência artificial que você pode incorporar em seus aplicativos. Você pode provisionar recursos individuais de serviços de IA do Azure para APIs específicas (por exemplo, **Linguagem** ou **Visão**), ou pode provisionar um único recurso dos **Serviços de IA do Azure** que forneça acesso a várias APIs de serviços de IA do Azure por meio de um único ponto de extremidade e chave. Nesse caso, você usará um único recurso dos **Serviços de IA do Azure**.

1. Abra o portal do Azure em `https://portal.azure.com` e entre usando a conta Microsoft associada à sua assinatura do Azure.
2. Na barra de pesquisa superior, pesquise *serviços de IA do Azure*, selecione **conta multisserviço dos serviços de IA do Azure** e crie um recurso com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*
    - **Grupo de recursos**: *escolha ou crie um grupo de recursos (se você estiver usando uma assinatura restrita, talvez não tenha permissão para criar um novo grupo de recursos; use o que foi fornecido)*
    - **Região**: *escolha uma região disponível*
    - **Nome**: *insira um nome exclusivo*
    - **Tipo de preço**: Standard S0
3. Marque as caixas de seleção necessárias e crie o recurso.
4. Aguarde a conclusão da implantação e veja os detalhes da implantação.
5. Acesse o recurso e exiba a página **Chaves e Ponto de Extremidade** do seu recurso. Esta página contém as informações que você precisará para se conectar ao seu recurso e usá-lo a partir de aplicativos que você desenvolve. Especificamente:
    - Um ponto de extremidade* HTTP *para o qual os aplicativos cliente podem enviar solicitações.
    - Duas *chaves *que podem ser usadas para autenticação (os aplicativos cliente podem usar qualquer uma das chaves para autenticar).
    - O *local* em que o recurso é hospedado. Isso é necessário para solicitações para algumas (mas não todas) APIs.

## Usar uma interface do usuário

As APIs de serviços de IA do Azure são baseadas em REST, portanto, você pode consumi-las enviando solicitações JSON por HTTP. Neste exemplo, você explorará um aplicativo de console que usa a API REST de **Linguagem** para executar a detecção de idioma, mas o princípio básico é o mesmo para todas as APIs com suporte no recurso Serviços de IA do Azure.

> **Nota**: Neste exercício, você pode optar por usar a API REST de **C#** ou **Python**. Nas etapas abaixo, execute as ações apropriadas para o idioma de sua preferência.

1. No Visual Studio Code, expanda a pasta **C-Sharp** ou **Python**, dependendo da sua preferência de linguagem de programação.
2. Exiba o conteúdo da pasta **rest-client** e observe que ela contém um arquivo para definições de configuração:

    - **C#**: appsettings.json
    - **Python**: .env

    Abra o arquivo de configuração e atualize os valores de configuração para refletir o **ponto de extremidade** e uma **chave** de autenticação para seu recurso de Serviços de IA do Azure. Salve suas alterações.

3. Observe que a pasta **rest-client** contém um arquivo de código para o aplicativo cliente:

    - **C#**: Program.cs
    - **Python**: rest-client.py

    Abra o arquivo de código e revise o código que ele contém, observando os seguintes detalhes:
    - Vários namespaces são importados para habilitar a comunicação HTTP
    - O código na função **Principal** recupera o ponto de extremidade e a chave para seu recurso de serviços de IA do Azure - eles serão usados para enviar solicitações REST para o serviço de Análise de Texto.
    - O programa aceita a entrada do usuário e usa a função **GetLanguage** para chamar a API REST de detecção de idioma da Análise de Texto para o ponto de extremidade dos serviços de IA do Azure para detectar o idioma do texto inserido.
    - A solicitação enviada à API consiste em um objeto JSON contendo os dados de entrada. Neste caso, uma coleção de objetos de **documento**,cada um dos quais com uma **id** e um **texto**.
    - A chave do serviço está incluída no cabeçalho da solicitação para autenticar o aplicativo cliente.
    - A resposta do serviço é um objeto JSON, que o aplicativo cliente pode analisar.

4. Clique com o botão direito do mouse na pasta **rest-client**, selecione *Abrir no Terminal Integrado* e execute o seguinte comando:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    pip install python-dotenv
    python rest-client.py
    ```

5. Quando solicitado, insira algum texto e revise o idioma detectado pelo serviço, que é retornado na resposta JSON. Por exemplo, tente digitar “Hello”, “Bonjour” e “Gracias”.
6. Quando terminar de testar o aplicativo, digite “sair” para parar o programa.

## Usar um SDK

Você pode escrever um código que consome as APIs REST dos serviços de IA do Azure diretamente, mas há Software Development Kits (SDKs) para muitas linguagens de programação populares, incluindo Microsoft C#, Python, Java e Node.js. O uso de um SDK pode simplificar muito o desenvolvimento de aplicativos que consomem serviços de IA do Azure.

1. No Visual Studio Code, expanda a pasta **sdk-client** sob a pasta **C-Sharp** ou **Python**, dependendo da linguagem de programação escolhida. Em seguida, execute `cd ../sdk-client` para alterar para a pasta **sdk-client **relevante.

2. Instale o pacote do SDK de Análise de Texto executando o comando apropriado para sua preferência de idioma:

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    ```

3. Exiba o conteúdo da pasta **sdk-client** e observe que ela contém um arquivo para definições de configuração:

    - **C#**: appsettings.json
    - **Python**: .env

    Abra o arquivo de configuração e atualize os valores de configuração para refletir o **ponto de extremidade** e uma **chave** de autenticação para seu recurso de Serviços de IA do Azure. Salve suas alterações.
    
4. Observe que a pasta **sdk-client** contém um arquivo de código para o aplicativo cliente:

    - **C#**: Program.cs
    - **Python**: sdk-client.py

    Abra o arquivo de código e revise o código que ele contém, observando os seguintes detalhes:
    - O namespace para o SDK instalado é importado
    - O código na função **Principal** recupera o ponto de extremidade e a chave para seu recurso de serviços de IA do Azure. Eles serão usados com o SDK para criar um cliente para o serviço de Análise de Texto.
    - A função **GetLanguage** usa o SDK para criar um cliente para o serviço e, em seguida, usa o cliente para detectar o idioma do texto que foi inserido.

5. Volte ao terminal, verifique se você está na pasta **sdk-client** e insira o seguinte comando para executar o programa:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python sdk-client.py
    ```

6. Quando solicitado, insira algum texto e revise o idioma detectado pelo serviço. Por exemplo, tente digitar “Adeus”, “Au revoir” e “Hasta la vista”.
7. Quando terminar de testar o aplicativo, digite “sair” para parar o programa.

> **Nota**: Alguns idiomas que exigem conjuntos de caracteres Unicode podem não ser reconhecidos neste aplicativo de console simples.

## Limpar os recursos

Se você não estiver usando os recursos do Azure criados neste laboratório para outros módulos de treinamento, poderá excluí-los para evitar incorrer em cobranças adicionais.

1. Abra o portal do Azure em `https://portal.azure.com`,e na barra de pesquisa superior, procure os recursos criados neste laboratório.

2. Na página de recursos, selecione **Excluir** e siga as instruções para excluir o recurso. Como alternativa, você pode excluir todo o grupo de recursos para limpar todos os recursos ao mesmo tempo.

## Mais informações

Para obter mais informações sobre os Serviços de IA do Azure, confira a [documentação dos Serviços de IA do Azure](https://docs.microsoft.com/azure/ai-services/what-are-ai-services).
