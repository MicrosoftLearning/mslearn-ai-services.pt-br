---
lab:
  title: Implementar a Segurança de Conteúdo de IA do Azure
---

# Implementar a Segurança de Conteúdo de IA do Azure

Neste exercício, você provisionará um recurso de Segurança de Conteúdo, testará o recurso no Estúdio de IA do Azure e testará o recurso no código.

## Provisionar um recurso de *Segurança de Conteúdo*

Caso ainda não tenha um na sua assinatura do Azure, será necesssário provisionar um recurso de **Segurança de Conteúdo**.

1. Abra o portal do Azure em `https://portal.azure.com` e entre usando a conta Microsoft associada à sua assinatura do Azure.
1. Selecione **Criar um recurso**.
1. No campo de pesquisa, pesquise **Segurança de Conteúdo**. Em seguida, nos resultados, escolha **Criar** em **Segurança de Conteúdo de IA do Azure**.
1. Provisione o recurso usando as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*.
    - **Grupo de recursos**: *crie ou escolha um grupo de recursos*.
    - **Região**: escolha **Leste dos EUA**.
    - **Nome**: *insira um nome exclusivo*.
    - **Tipo de preço**: selecione **F0** (*gratuito*), ou **S** (*Standard*) se F0 não estiver disponível.
1. Selecione **Revisar + criar** e, em seguida, selecione **Criar** para provisionar o recurso.
1. Aguarde a conclusão da implantação e acesse o recurso.
1. Selecione **Controle de Acesso** na barra de navegação à esquerda, clique em **+ Adicionar** e **Adicionar atribuição de função**.
1. Role para baixo, escolha a função **Usuário dos Serviços Cognitivos** e clique em **Avançar**.
1. Adicione sua conta a essa função e clique em **Revisar + atribuir**.
1. Na barra de navegação esquerda, escolha **Gerenciamento de Recursos** e **Chaves e Ponto de Extremidade**. Deixe esta página aberta para que você possa copiar as chaves mais tarde.

## Usar Proteções de Prompt da Segurança de Conteúdo de IA do Azure

Neste exercício, você usará o Estúdio de IA do Azure para testar as Proteções de Prompt da Segurança de Conteúdo com duas entradas de exemplo. Uma simula um prompt do usuário e a outra simula um documento com texto potencialmente inseguro incorporado a ele.

1. Em outra guia do navegador, abra a página de Segurança de Conteúdo do [Estúdio de IA do Azure](https://ai.azure.com/explore/contentsafety) e conecte-se.
1. Escolha **Experimentar** em **Moderar conteúdo de texto**.
1. Na página **Moderar conteúdo de texto**, em **Serviços de IA do Azure**, escolha o recurso de Segurança de Conteúdo que você criou anteriormente.
1. Escolha **Várias categorias de risco em uma frase**. Revise o texto do documento em busca de possíveis problemas.
1. Escolha **Executar teste** e examine os resultados.
1. Você também pode alterar os níveis de limite e clicar em **Executar teste** novamente.
1. Na barra de navegação à esquerda, clique em **Detecção de material protegido para texto**.
1. Escolha **Letra protegida** e observe que se trata da letra de uma música publicada.
1. Escolha **Executar teste** e examine os resultados.
1. Na barra de navegação à esquerda, escolha **Moderar conteúdo da imagem**.
1. Clique em **Conteúdo de automutilação**.
1. Observe que todas as imagens são desfocadas por padrão no Estúdio de IA. Você também deve estar ciente de que o conteúdo sexual nas amostras é muito leve.
1. Escolha **Executar teste** e examine os resultados.
1. Clique em **Proteções de prompt** na barra de navegação à esquerda
1. Na página **Proteções de prompt**, em **Serviços de IA do Azure**, selecione o recurso de Segurança de Conteúdo que você criou anteriormente.
1. Selecione **Conteúdo de ataque no prompt e no documento**. Revise o prompt do usuário e o texto do documento em busca de possíveis problemas.
1. Selecione **Executar teste**.
1. Em **Exibir resultados**, verifique se os ataques de jailbreak foram detectados no prompt do usuário e no documento.

    > [!TIP]
    > O código está disponível para todos os exemplos no Estúdio de IA.

1. Em **Próximas etapas**, em **Exibir o código**, selecione **Exibir código**. A janela **Código de exemplo** será exibida.
1. Use a seta para baixo para selecionar Python ou C# e, em seguida, selecione **Copiar** para copiar o código de exemplo para a área de transferência.
1. Feche a tela **Código de exemplo**.

### Configurar seu aplicativo

Agora você criará um aplicativo em C# ou Python.

#### C#

##### Pré-requisitos

* [Visual Studio Code](https://code.visualstudio.com/) em uma das [plataformas compatíveis](https://code.visualstudio.com/docs/supporting/requirements#_platforms).
* O [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) é a estrutura de destino para este exercício.
* A [Extensão do C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) para Visual Studio Code.

##### Configurando

Execute as etapas a seguir para preparar o Visual Studio Code para o exercício.

1. Inicie o Visual Studio Code e, no modo de exibição Explorer, clique em **Criar Projeto do .NET** selecionando **Aplicativo de console**.
1. Escolha uma pasta no computador e dê um nome ao projeto. Clique em **Criar projeto** e confirme a mensagem de aviso.
1. No painel Explorer, expanda Gerenciador de Soluções e selecione **Program.cs**.
1. Compile e execute o projeto clicando em **Executar** -> **Executar sem depurar**. 
1. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto em C# e selecione **Adicionar Pacote NuGet**.
1. Pesquise **Azure.AI.TextAnalytics** e escolha a versão mais recente.
1. Pesquise um segundo pacote NuGet: **Microsoft.Extensions.Configuration.Json 8.0.0**. O arquivo de projeto listará dois pacotes NuGet.

##### Incluir código

1. Cole o código de exemplo que você copiou anteriormente na seção **ItemGroup**.
1. Role para baixo para encontrar *Substituir por your_subscription_key e ponto de extremidade*.
1. No portal do Azure, na página Chaves e Ponto de Extremidade, copie uma das Chaves (1 ou 2). Substitua **<your_subscription_key>** por esse valor.
1. No portal do Azure, na página Chaves e Ponto de Extremidade, copie o Ponto de Extremidade. Cole esse valor em seu código para substituir **<your_endpoint_value>**.
1. No **Estúdio de IA do Azure**, copie o valor do **Prompt do usuário**. Cole-o em seu código para substituir **<test_user_prompt>**.
1. Role para baixo até **<this_is_a_document_source>** e exclua esta linha de código.
1. No **Estúdio de IA do Azure**, copie o valor do **Documento**.
1. Role para baixo até **<this_is_another_document_source>** e cole o valor do documento.
1. Clique em **Executar** -> **Executar sem depurar** e verifique se um ataque foi detectado. 

#### Python

##### Pré-requisitos

* [Visual Studio Code](https://code.visualstudio.com/) em uma das [plataformas compatíveis](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* A [extensão do Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) foi instalado para o Visual Studio Code.

* O [módulo de solicitações](https://pypi.org/project/requests/) foi instalado.

1. Crie um novo arquivo Python com uma extensão **.py** e dê a ele um nome adequado.
1. Cole o código de exemplo que você copiou anteriormente.
1. Role para baixo para encontrar a seção intitulada *Substituir por your_own_subscription_key e endpoint*.
1. No portal do Azure, na página Chaves e Ponto de Extremidade, copie uma das Chaves (1 ou 2). Substitua **<your_subscription_key>** por esse valor.
1. No portal do Azure, na página Chaves e Ponto de Extremidade, copie o Ponto de Extremidade. Cole esse valor em seu código para substituir **<your_endpoint_value>**.
1. No **Estúdio de IA do Azure**, copie o valor do **Prompt do usuário**. Cole-o em seu código para substituir **<test_user_prompt>**.
1. Role para baixo até **<this_is_a_document_source>** e exclua esta linha de código.
1. No **Estúdio de IA do Azure**, copie o valor do **Documento**.
1. Role para baixo até **<this_is_another_document_source>** e cole o valor do documento.
1. No terminal integrado do arquivo, execute o programa, por exemplo:

    - `.\prompt-shield.py`

1. Valide se um ataque foi detectado.
1. Você também pode experimentar diferentes conteúdos de teste e valores de documentos.
