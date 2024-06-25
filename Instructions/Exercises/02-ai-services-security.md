---
lab:
  title: Gerenciar a segurança dos Serviços de IA do Azure
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Gerenciar a segurança dos Serviços de IA do Azure

A segurança é uma consideração crítica para qualquer aplicativo e, como desenvolvedor, você deve garantir que o acesso a recursos, como os serviços de IA do Azure, seja restrito apenas àqueles que precisam dele.

O acesso aos serviços de IA do Azure normalmente é controlado por meio de chaves de autenticação, que são geradas quando você cria inicialmente um recurso de serviços de IA do Azure.

## Clonar o repositório no Visual Studio Code

Você desenvolverá seu código usando o Visual Studio Code. Os arquivos de código para seu aplicativo foram fornecidos em um repositório do GitHub.

> **Dica**: Se você já clonou o repositório **mslearn-ai-services** recentemente, abra-o no Visual Studio Code. Caso contrário, siga estas etapas para cloná-lo em seu ambiente de desenvolvimento.

1. Inicie o Visual Studio Code.
2. Abra a paleta (SHIFT+CTRL+P) e execute o comando **Git: Clone** para clonar o repositório `https://github.com/MicrosoftLearning/mslearn-ai-services` em uma pasta local (não importa qual pasta).
3. Depois que o repositório for clonado, abra a pasta no Visual Studio Code.
4. Aguarde enquanto arquivos adicionais são instalados para dar suporte aos projetos de código C# no repositório, se necessário

    > **Observação**: se você for solicitado a adicionar ativos necessários para compilar e depurar, selecione **Agora não**.

5. Expanda a pasta `Labfiles/02-ai-services-security`.

Os códigos tanto em C# quanto em Python foram fornecidos. Expanda a pasta da linguagem escolhida.

## Provisionar um recurso dos Serviços de IA do Azure

Caso ainda não tenha um na sua assinatura, provisione um recurso dos **Serviços de IA do Azure**.

1. Abra o portal do Azure em `https://portal.azure.com` e entre usando a conta Microsoft associada à sua assinatura do Azure.
2. Na barra de pesquisa superior, pesquise *serviços de IA do Azure*, selecione **Serviços de IA do Azure** e crie um recurso de conta multisserviço dos serviços de IA do Azure com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*
    - **Grupo de recursos**: *escolha ou crie um grupo de recursos (se você estiver usando uma assinatura restrita, talvez não tenha permissão para criar um novo grupo de recursos; use o que foi fornecido)*
    - **Região**: *escolha uma região disponível*
    - **Nome**: *insira um nome exclusivo*
    - **Tipo de preço**: Standard S0
3. Marque as caixas de seleção necessárias e crie o recurso.
4. Aguarde a conclusão da implantação e veja os detalhes da implantação.

## Gerenciar chaves de autenticação

Quando você criou seu recurso de serviços de IA do Azure, duas chaves de autenticação foram geradas. Você pode gerenciá-las no portal do Azure ou usando a CLI (Command Line Interface, interface de linha de comando) do Azure.

1. No portal do Azure, acesse o recurso de serviços de IA do Azure e visualize a página de **chaves e ponto de extremidade**. Esta página contém as informações que você precisará para se conectar ao seu recurso e usá-lo a partir de aplicativos que você desenvolve. Especificamente:
    - Um *ponto de extremidade* HTTP para o qual os aplicativos cliente podem enviar solicitações.
    - Duas *chaves* que podem ser usadas para autenticação (aplicativos cliente podem usar qualquer uma das chaves. Uma prática comum é usar uma para desenvolvimento e outra para produção. Você pode facilmente regenerar a chave de desenvolvimento depois que os desenvolvedores terminarem seu trabalho para impedir o acesso contínuo).
    - O *local* em que o recurso é hospedado. Isso é necessário para solicitações para algumas (mas não todas) APIs.
2. Agora você pode usar o comando a seguir para obter a lista de chaves de serviços de IA do Azure, substituindo *&lt;resourceName&gt;* pelo nome do seu recurso de serviços de IA do Azure e *&lt;resourceGroup&gt;* pelo nome do grupo de recursos no qual você o criou.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    O comando retorna uma lista das chaves para seu recurso de serviços de IA do Azure — há duas chaves, chamadas **key1** e **key2**.

    > **Dica**: Se você ainda não autenticou a CLI do Azure, execute `az login` e entre em sua conta.

3. Para testar seu serviço de IA do Azure, você pode usar **curl** — uma ferramenta de linha de comando para solicitações HTTP. Na pasta **02-ai-services-security**, abra **rest-test.cmd** e edite o comando **curl** nela contido (mostrado abaixo), substituindo *&lt;yourEndpoint&gt;* e *&lt;yourKey&gt;* pelo URI do ponto de extremidade e a chave **Key1** para usar a API de análise de texto em seu recurso dos serviços de IA do Azure.

    ```bash
    curl -X POST "<yourEndpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: 81468b6728294aab99c489664a818197" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

4. Salve as alterações e execute o seguinte comando:

    ```
    ./rest-test.cmd
    ```

O comando retorna um documento JSON contendo informações sobre o idioma detectado nos dados de entrada (que deve ser inglês).

5. Se uma chave for comprometida ou os desenvolvedores que a possuem não precisarem mais de acesso, você poderá gerá-la novamente no portal ou usando a CLI do Azure. Execute o comando a seguir para gerar novamente sua **chave key1** (substituindo *&lt;resourceName&gt;* e *&lt;resourceGroup&gt;* pelo seu recurso).

    ```
    az cognitiveservices account keys regenerate --name <resourceName> --resource-group <resourceGroup> --key-name key1
    ```

A lista de chaves para seu recurso de serviços de IA do Azure é retornada - observe que a **key1** foi alterada desde a última vez que você as recuperou.

6. Execute novamente o comando **rest-test** com a chave antiga (você pode usar a seta **^** no teclado para percorrer os comandos anteriores) e verifique se ele agora falha.
7. Edite o comando *curl* em **rest-test.cmd** substituindo a chave pelo novo valor **key1** e salve as alterações. Em seguida, execute novamente o comando **rest-test** e verifique se ele foi bem-sucedido.

> **Dica**: neste exercício, você usou os nomes completos dos parâmetros da CLI do Azure, como **--resource-group**.  Você também pode usar alternativas mais curtas, como **-g**, para tornar seus comandos menos detalhados (mas um pouco mais difíceis de entender).  A [referência do comando da CLI dos Serviços de IA do Azure](https://docs.microsoft.com/cli/azure/cognitiveservices?view=azure-cli-latest) lista as opções de parâmetro para cada comando da CLI dos Serviços de IA do Azure.

## Proteger o acesso por chave com o Azure Key Vault

Você pode desenvolver aplicativos que consomem serviços de IA do Azure usando uma chave para autenticação. No entanto, isso significa que o código do aplicativo deve ser capaz de obter a chave. Uma opção é armazenar a chave em uma variável de ambiente ou em um arquivo de configuração onde o aplicativo é implantado, mas essa abordagem deixa a chave vulnerável a acesso não autorizado. Uma abordagem melhor ao desenvolver aplicativos no Azure é armazenar a chave com segurança no Azure Key Vault e fornecer acesso à chave por meio de uma *identidade gerenciada *(em outras palavras, uma conta de usuário usada pelo próprio aplicativo).

### Criar um cofre de chave e definir um segredo

Primeiro, você precisa criar um cofre de chaves e adicionar um *segredo* para a chave de serviços de IA do Azure.

1. Anote o valor **key1** para seu recurso de serviços de IA do Azure (ou copie-o para a área de transferência).
2. No portal do Azure, na **Página inicial**, selecione **&#65291; crie um botão de recurso**, procure por *cofre de chaves* e crie um recurso de **cofre de chaves** com as seguintes configurações:

    - Guia **Básico**
        - **Assinatura**: *sua assinatura do Azure*
        - **Grupo de recursos**: *o mesmo grupo de recursos do recurso de serviços de IA do Azure *
        - **Nome do cofre de chaves**: *Insira um nome único*
        - **Região**: *a mesma região do recurso de serviços de IA do Azure*
        - **Tipo de preço**: padrão
    
    - Guia de **Configuração de acesso**
        -  **Modelo de permissão**: política de acesso ao cofre
        - Role para baixo até a seção de **políticas de acesso** e selecione seu usuário usando a caixa de verificação à esquerda. Em seguida, selecione **Examinar + criar** e selecione **Criar** para criar o seu recurso.
     
3. Aguarde a conclusão da implantação e acesse o recurso do cofre de chaves.
4. No painel de navegação à esquerda, selecione **Segredos** (na seção Objetos).
5. Selecione **+ Gerar/Importar** e adicione um novo segredo com as seguintes configurações:
    - **Opções de upload**: Manual.
    - **Nome**: AI-Services-Key *(é importante corresponder exatamente a isso, pois posteriormente você executará o código que recupera o segredo com base neste nome)*
    - **Valor**: *Sua chave de serviços de IA do Azure **key1***
6. Selecione **Criar**.

### Criar uma entidade de serviço

Para acessar o segredo no cofre de chaves, seu aplicativo deve usar uma entidade de serviço que tenha acesso ao segredo. Você usará a CLI do Azure para criar a entidade de serviço, localizar sua ID de objeto e conceder acesso ao segredo no Cofre do Azure.

1. Execute o seguinte comando da CLI do Azure, substituindo *&lt;spName&gt;* por um nome adequado exclusivo para uma identidade de aplicativo (por exemplo, *ai-app* com suas iniciais acrescentadas no final; o nome deve ser exclusivo em seu locatário). Substitua também *&lt;subscriptionId&gt;* e *&lt;resourceGroup&gt;* pelos valores corretos para sua ID de assinatura e o grupo de recursos que contém seus serviços de IA do Azure e os recursos de cofre de chaves:

    > **Dica**: se você não tiver certeza de sua ID de assinatura, use o comando da **conta az mostrado** para recuperar suas informações de assinatura — a ID de assinatura é o atributo de** id** na saída. Se você vir um erro sobre o objeto já existente, escolha um nome exclusivo diferente.

    ```
    az ad sp create-for-rbac -n "api://<spName>" --role owner --scopes subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
    ```

A saída desse comando inclui informações sobre sua nova entidade de serviço. Ela deve ser semelhante a este:

    ```
    {
        "appId": "abcd12345efghi67890jklmn",
        "displayName": "api://ai-app-",
        "password": "1a2b3c4d5e6f7g8h9i0j",
        "tenant": "1234abcd5678fghi90jklm"
    }
    ```

Anote os valores de **appId**, **senha** e **locatário** – você precisará deles mais tarde (se fechar este terminal, você não poderá recuperar a senha; portanto, é importante tomar nota dos valores agora – você pode colar a saída em um novo arquivo de texto em seu computador local para garantir que possa encontrar os valores necessários mais tarde!)

2. Para obter a **ID de objeto** da sua entidade de serviço, execute o seguinte comando da CLI do Azure, substituindo *&lt;appId&gt;* pelo valor da sua ID de aplicativo da entidade de serviço.

    ```
    az ad sp show --id <appId>
    ```

3. Copie o valor `id` no JSON retornado em resposta. 
3. Para atribuir permissão para que a sua nova entidade de serviço acesse segredos em seu Key Vault, execute o seguinte comando da CLI do Azure, substituindo *&lt;keyVaultName&gt;* pelo nome do recurso do Azure Key Vault e *&lt;objectId&gt;* pelo valor do valor da ID da entidade de serviço que você acabou de copiar.

    ```
    az keyvault set-policy -n <keyVaultName> --object-id <objectId> --secret-permissions get list
    ```

### Usar a entidade de serviço em um aplicativo

Agora você está pronto para usar a identidade da entidade de serviço em um aplicativo, para que ele possa acessar a chave secreta dos serviços de IA do Azure em seu cofre de chaves e usá-la para se conectar ao seu recurso de serviços de IA do Azure.

> **Nota**: Neste exercício, armazenaremos as credenciais da entidade de serviço na configuração do aplicativo e as usaremos para autenticar uma **identidade ClientSecretCredential** no código do aplicativo. Isso é bom para desenvolvimento e teste, mas em um aplicativo de produção real, um administrador atribuiria uma *identidade gerenciada* ao aplicativo para que ele use a identidade da entidade de serviço para acessar recursos, sem armazenar em cache ou armazenar a senha.

1. No terminal, alterne para a pasta **C-Sharp** ou **Python**, dependendo da sua preferência de idioma, executando `cd C-Sharp` ou `cd Python`. Em seguida, execute `cd keyvault_client` para navegar até a pasta do aplicativo.
2. Instale os pacotes que você precisará usar para o Azure Key Vault e a API de Análise de Texto em seu recurso dos serviços de IA do Azure executando o comando apropriado para a sua preferência de idioma:

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    dotnet add package Azure.Identity --version 1.5.0
    dotnet add package Azure.Security.KeyVault.Secrets --version 4.2.0-beta.3
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    pip install azure-identity==1.5.0
    pip install azure-keyvault-secrets==4.2.0
    ```

3. Exiba o conteúdo da pasta **keyvault-client** e observe que ela contém um arquivo para definições de configuração:
    - **C#**: appsettings.json
    - **Python**: .env

    Abra o arquivo de configuração e atualize os valores de configuração que ele contém para refletir as seguintes configurações:
    
    - O **ponto de extremidade** para o recurso de serviços de IA do Azure
    - O nome do recurso do **cofre de chaves do Azure**
    - O **inquilino** da entidade de serviço
    - O ** appId** para a entidade de serviço
    - A **password** para a entidade de serviço

     Salve suas alterações pressionando **CTRL+S**.
4. Observe que a pasta **keyvault-client** contém um arquivo de código para o aplicativo cliente:

    - **C#**: Program.cs
    - **Python**: keyvault-client.py

    Abra o arquivo de código e revise o código que ele contém, observando os seguintes detalhes:
    - O namespace para o SDK instalado é importado
    - O código na função **Principal** recupera as definições de configuração do aplicativo e, em seguida, usa as credenciais da entidade de serviço para obter a chave de serviços de IA do Azure do cofre de chaves.
    - A função **GetLanguage** usa o SDK para criar um cliente para o serviço e, em seguida, usa o cliente para detectar o idioma do texto que foi inserido.
5. Insira o seguinte comando para executar o programa:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python keyvault-client.py
    ```

6. Quando solicitado, insira algum texto e revise o idioma detectado pelo serviço. Por exemplo, tente digitar “Hello”, “Bonjour” e “Gracias”.
7. Quando terminar de testar o aplicativo, digite “sair” para parar o programa.

## Limpar os recursos

Se você não estiver usando os recursos do Azure criados neste laboratório para outros módulos de treinamento, poderá excluí-los para evitar incorrer em cobranças adicionais.

1. Abra o portal do Azure em `https://portal.azure.com`,e na barra de pesquisa superior, procure os recursos criados neste laboratório.

2. Na página de recursos, selecione **Excluir** e siga as instruções para excluir o recurso. Como alternativa, você pode excluir todo o grupo de recursos para limpar todos os recursos ao mesmo tempo.

## Mais informações

Para obter mais informações sobre como proteger os serviços de IA do Azure, consulte a [documentação de segurança dos serviços de IA do Azure](https://docs.microsoft.com/azure/ai-services/security-features).
