---
lab:
  title: Usar contêineres dos Serviços de IA do Azure
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Usar contêineres dos Serviços de IA do Azure

O uso dos serviços de IA do Azure hospedados no Azure permite que os desenvolvedores de aplicativos se concentrem na infraestrutura de seu próprio código, enquanto se beneficiam de serviços escalonáveis gerenciados pela Microsoft. No entanto, em muitos cenários, as organizações exigem mais controle sobre sua infraestrutura de serviços e os dados que são passados entre os serviços.

Muitas das APIs de serviços de IA do Azure podem ser empacotadas e implantadas em um *contêiner*, permitindo que as organizações hospedem os serviços de IA do Azure em sua própria infraestrutura, por exemplo, em servidores Docker locais, Instâncias de Contêiner do Azure ou clusters dos Serviços de Kubernetes do Azure. Os serviços de IA do Azure em contêineres precisam se comunicar com uma conta de serviços de IA do Azure baseada no Azure para dar suporte à cobrança; mas os dados do aplicativo não são passados para o serviço back-end, e as organizações têm maior controle sobre a configuração de implantação de seus contêineres, permitindo soluções personalizadas para autenticação, escalabilidade e outras considerações.

> **Observação**: há um problema sendo investigado no momento em que alguns usuários atingem onde os contêineres não são implantados corretamente e as chamadas para esses contêineres falham. As atualizações neste laboratório serão feitas assim que o problema for resolvido.

## Clonar o repositório no Visual Studio Code

Você desenvolverá seu código usando o Visual Studio Code. Os arquivos de código para seu aplicativo foram fornecidos em um repositório do GitHub.

> **Dica**: Se você já clonou o repositório **mslearn-ai-services**, abra-o no Visual Studio Code. Caso contrário, siga estas etapas para cloná-lo em seu ambiente de desenvolvimento.

1. Inicie o Visual Studio Code.
2. Abra a paleta (SHIFT+CTRL+P) e execute o comando **Git: Clone** para clonar o repositório `https://github.com/MicrosoftLearning/mslearn-ai-services` em uma pasta local (não importa qual pasta).
3. Depois que o repositório for clonado, abra a pasta no Visual Studio Code.
4. Aguarde enquanto arquivos adicionais são instalados para dar suporte aos projetos de código C# no repositório, se necessário

    > **Observação**: se você for solicitado a adicionar ativos necessários para compilar e depurar, selecione **Agora não**.

5. Expanda a pasta `Labfiles/04-use-a-container`.

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
5. Quando o recurso tiver sido implantado, vá até ele e exiba sua página **Chaves e Ponto de Extremidade**. Você precisará do ponto de extremidade e de uma das chaves desta página no próximo procedimento.

## Implantar e executar um contêiner de análise de texto

Muitas APIs de serviços de IA do Azure usadas com frequência estão disponíveis em imagens de contêiner. Para obter uma lista completa, confira a [documentação dos serviços de IA do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#container-availability-in-azure-cognitive-services) Neste exercício, você usará a imagem de contêiner para a API de *detecção de idioma* de Análise de Texto, mas os princípios são os mesmos para todas as imagens disponíveis.

1. No portal do Azure, na **Página inicial**, selecione **&#65291; Crie um botão de recurso**, procure *instâncias de contêiner* e crie um recurso de **instâncias de contêiner** com as seguintes configurações:

    - **Noções básicas**:
        - **Assinatura**: *sua assinatura do Azure*
        - **Grupo de recursos**: *escolha o grupo de recursos que contém seu recurso de serviços de IA do Azure*
        - **Nome do contêiner**: *insira um nome exclusivo*
        - **Região**: *escolha uma região disponível*
        - **Fonte da imagem**: outro registro
        - **Tipo de imagem**: público
        - **Imagem**: `mcr.microsoft.com/azure-cognitive-services/textanalytics/language:latest`
        - **Tipo de sistema operacional**: Linux
        - **Tamanho**: 1 vcpu, memória de 12 GB
    - **Rede**:
        - **Tipo de rede**: pública
        - **Rótulo de nome DNS**: *insira um nome exclusivo para o ponto de extremidade do contêiner*
        - **Portas**: *Altere a porta TCP de 80 para **5000***
    - **Avançado:**
        - **Política de reinicialização**: em caso de falha
        - **Variáveis de ambiente**:

            | Marcar como seguro | Chave | Valor |
            | -------------- | --- | ----- |
            | Sim | `ApiKey` | *Qualquer uma das chaves para seu recursos de serviços de IA do Azure* |
            | Sim | `Billing` | *A URI do ponto de extremidade para seu recurso de serviços de IA do Azure * |
            | Não | `Eula` | `accept` |

        - **Substituição de comando**: [ ]
    - **Tags:**
        - *Não adicione nenhuma tag*

2. Selecione **Revisar + criar** e **Criar**. Aguarde a conclusão da implantação e acesse o recurso implantado.
    > **Observação** Observe que a implantação de um contêiner de IA do Azure em Instâncias de Contêiner do Azure normalmente leva de 5 a 10 minutos (provisionamento) antes de estarem prontas para uso.
3. Observe as seguintes propriedades do recurso de instância de contêiner em sua página **Visão geral**:
    - **Status**: isso deve estar *em execução*.
    - **Endereço** IP: este é o endereço IP público que você pode usar para acessar suas instâncias de contêiner.
    - **FQDN**: este é o *nome de domínio totalmente qualificado* do recurso de instâncias de contêiner, você pode usá-lo para acessar as instâncias de contêiner em vez do endereço IP.

    > **Observação**: neste exercício, você implantou a imagem de contêiner dos serviços de IA do Azure para tradução de texto em um recurso ACI (Azure Container Instances, Instâncias de Contêiner do Azure). Você pode usar uma abordagem semelhante para implantá-lo em um host do *[Docker](https://www.docker.com/products/docker-desktop)* em seu próprio computador ou rede executando o seguinte comando (em uma única linha) para implantar o contêiner de detecção de idioma em sua instância local do Docker, substituindo *&lt;yourEndpoint&gt;* e *&lt;yourKey&gt;* pela URI do ponto de extremidade e qualquer uma das chaves para o recurso de serviços de IA do Azure.
    > O comando procurará a imagem em sua máquina local e, se não a encontrar lá, a extrairá do registro de imagem *mcr.microsoft.com* e a implantará em sua instância do Docker. Quando a implantação estiver concluída, o contêiner será iniciado e escutará as solicitações de entrada na porta 5000.

    ```
    docker run --rm -it -p 5000:5000 --memory 12g --cpus 1 mcr.microsoft.com/azure-cognitive-services/textanalytics/language:latest Eula=accept Billing=<yourEndpoint> ApiKey=<yourKey>
    ```

## Usar o contêiner

1. No editor, abra **rest-test.cmd** e edite o comando **curl** contido (mostrado abaixo), substituindo *&lt;your_ACI_IP_address_or_FQDN&gt;* pelo endereço IP ou FQDN para seu contêiner.

    ```
    curl -X POST "http://<your_ACI_IP_address_or_FQDN>:5000/text/analytics/v3.0/languages" -H "Content-Type: application/json" --data-ascii "{'documents':[{'id':1,'text':'Hello world.'},{'id':2,'text':'Salut tout le monde.'}]}"
    ```

2. Salve suas alterações no script pressionando **CTRL+S**. Observe que você não precisa especificar o ponto de extremidade ou a chave dos serviços de IA do Azure. A solicitação é processada pelo serviço em contêiner. O contêiner, por sua vez, se comunica periodicamente com o serviço no Azure para relatar o uso para cobrança, mas não envia dados de solicitação.
3. Execute o comando a seguir para executar o script:

    ```
    ./rest-test.cmd
    ```

4. Verifique se o comando retorna um documento JSON contendo informações sobre o idioma detectado nos dois documentos de entrada (que devem ser inglês e francês).

## Limpeza

Se você tiver terminado de experimentar sua instância de contêiner, exclua-a.

1. No portal do Azure, abra o grupo de recursos onde você criou seus recursos para este exercício.
2. Selecione o recurso de instância de contêiner e exclua-o.

## Limpar os recursos

Se você não estiver usando os recursos do Azure criados neste laboratório para outros módulos de treinamento, poderá excluí-los para evitar incorrer em cobranças adicionais.

1. Abra o portal do Azure em `https://portal.azure.com`,e na barra de pesquisa superior, procure os recursos criados neste laboratório.

2. Na página de recursos, selecione **Excluir** e siga as instruções para excluir o recurso. Como alternativa, você pode excluir todo o grupo de recursos para limpar todos os recursos ao mesmo tempo.

## Mais informações

Para obter mais informações sobre contêineres dos serviços de IA do Azure, consulte a [documentação dos contêineres dos serviços de IA do Azure](https://learn.microsoft.com/azure/ai-services/cognitive-services-container-support)
