---
lab:
  title: Monitorar os Serviços de IA do Azure
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Monitorar os Serviços de IA do Azure

Os Serviços de IA do Azure podem ser uma parte crítica de uma infraestrutura geral de aplicativos. É importante ser capaz de monitorar a atividade e ser alertado para problemas que podem precisar de atenção.

## Clonar o repositório no Visual Studio Code

Você desenvolverá seu código usando o Visual Studio Code. Os arquivos de código para seu aplicativo foram fornecidos em um repositório do GitHub.

> **Dica**: Se você já clonou o repositório **mslearn-ai-services**, abra-o no Visual Studio Code. Caso contrário, siga estas etapas para cloná-lo em seu ambiente de desenvolvimento.

1. Inicie o Visual Studio Code.
2. Abra a paleta (SHIFT+CTRL+P) e execute o comando **Git: Clone** para clonar o repositório `https://github.com/MicrosoftLearning/mslearn-ai-services` em uma pasta local (não importa qual pasta).
3. Depois que o repositório for clonado, abra a pasta no Visual Studio Code.
4. Aguarde enquanto arquivos adicionais são instalados para dar suporte aos projetos de código C# no repositório, se necessário

    > **Observação**: se você for solicitado a adicionar ativos necessários para compilar e depurar, selecione **Agora não**.

5. Expanda a pasta `Labfiles/03-monitor-ai-services`.

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
5. Quando o recurso tiver sido implantado, vá até ele e exiba sua página **Chaves e Ponto de Extremidade**. Anote a URI do ponto de extremidade - você precisará dela mais tarde.

## Configurar um alerta

Vamos começar a monitorar definindo uma regra de alerta para que você possa detectar a atividade em seu recurso de serviços de IA do Azure.

1. No portal Azure, acesse o recurso de serviços de IA do Azure e exiba sua página **Alertas** (na seção **Monitoramento**).
2. Selecione a lista suspensa **+ Criar** e selecione **Regra de alerta**
3. Na página **Criar uma regra de alerta**, em **Escopo**, verifique se o recurso de serviços de IA do Azure está listado. (Fechar **Selecione um painel de sinal** se aberto)
4. Selecione a guia **Condição** e o link **Veja todos os sinais** para mostrar o painel **Selecione um sinal** que aparece à direita, onde você pode selecionar um tipo de sinal a ser monitorado.
5. Na lista **Tipo de sinal**, role para baixo até a seção **Log de Atividades** do e selecione **Chaves de Lista (Conta de API dos Serviços Cognitivos)**. Em seguida, selecione **Aplicar**.
6. Revise a atividade das últimas 6 horas.
7. Selecione a guia **Ações**. Observe que você pode especificar um *grupo de ações*. Isso permite configurar ações automatizadas quando um alerta é disparado, por exemplo, enviar uma notificação por e-mail. Não faremos isso neste exercício; Mas pode ser útil fazer isso em um ambiente de produção.
8. Na guia **Detalhes**, defina o **nome da regra de alerta** como **Alerta de lista de chaves**.
9. Selecione **Examinar + criar**. 
10. Analise a configuração do cluster. Selecione **Criar** e aguarde até que o aplicativo Web seja criado.
11. Agora você pode usar o comando a seguir para obter a lista de chaves de serviços de IA do Azure, substituindo *&lt;resourceName&gt;* pelo nome do seu recurso de serviços de IA do Azure e *&lt;resourceGroup&gt;* pelo nome do grupo de recursos no qual você o criou.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    O comando retorna uma lista das chaves para seu recurso de serviços de IA do Azure.

    > **Observação** Se você não tiver feito logon na CLI do Azure, talvez seja necessário executar `az login` antes que o comando list keys funcione.

12. Volte para o navegador que contém o portal do Azure e atualize sua **página Alertas**. Você deve ver um **alerta Sev 4** listado na tabela (se não, aguarde até cinco minutos e atualize novamente).
13. Selecione o alerta para ver seus detalhes.

## Visualizar métricas

Além de definir alertas, você pode exibir métricas para seu recurso de serviços de IA do Azure para monitorar sua utilização.

1. No portal do Azure, na página do recurso de serviços de IA do Azure, selecione **Métricas** (na seção **Monitoramento**).
2. Se não houver nenhum gráfico, selecione **+ Novo gráfico**. Em seguida, na lista **Métricas** , revise as possíveis métricas que você pode visualizar e selecione **Total de chamadas**.
3. Na lista suspensa **Agregação**, selecione **Contagem**.  Isso permitirá que você monitore o total de chamadas para o recurso de Serviço Cognitivo, o que é útil para determinar quanto o serviço está sendo usado em um período.
4. Para gerar algumas solicitações para seu serviço de IA do Azure, você usará **curl** - uma ferramenta de linha de comando para solicitações HTTP. No editor, abra **rest-test.cmd** e edite o comando **curl** contido (mostrado abaixo), substituindo *&lt;yourEndpoint&gt;* e *&lt;yourKey&gt;* pelo URI do ponto de extremidade e **Key1** para usar a API de Análise de Texto em seu Recurso dos serviços de IA do Azure.

    ```
    curl -X POST "<your-endpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <your-key>" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

5. Salve as alterações, depois execute o seguinte comando:

    ```
    ./rest-test.cmd
    ```

    O comando retorna um documento JSON contendo informações sobre o idioma detectado nos dados de entrada (que deve ser inglês).

6. Execute novamente o comando **rest-test** várias vezes para gerar alguma atividade de chamada (você pode usar a **^** chave para percorrer os comandos anteriores).
7. Retorne à página **Métricas** no portal do Azure e atualize o gráfico de contagem **Total de Chamadas**. Pode levar alguns minutos para que as chamadas que você fez usando *curl* sejam refletidas no gráfico. Continue atualizando o gráfico até que ele seja atualizado para incluí-las.

## Limpar os recursos

Se você não estiver usando os recursos do Azure criados neste laboratório para outros módulos de treinamento, poderá excluí-los para evitar incorrer em cobranças adicionais.

1. Abra o portal do Azure em `https://portal.azure.com`,e na barra de pesquisa superior, procure os recursos criados neste laboratório.

2. Na página de recursos, selecione **Excluir** e siga as instruções para excluir o recurso. Como alternativa, você pode excluir todo o grupo de recursos para limpar todos os recursos ao mesmo tempo.

## Mais informações

Uma das opções para monitorar os serviços de IA do Azure é usar o *log de diagnóstico*. Depois de habilitado, o log de diagnóstico captura informações detalhadas sobre o uso de recursos dos serviços de IA do Azure e pode ser uma ferramenta útil de monitoramento e depuração. Pode levar mais de uma hora após a configuração do log de diagnóstico para gerar qualquer informação, e é por isso que não o exploramos neste exercício; mas você pode saber mais sobre isso na [documentação dos Serviços de IA do Azure](https://docs.microsoft.com/azure/ai-services/diagnostic-logging).