---
lab:
  title: Configuração do ambiente
  module: Setup
---

# Configuração do ambiente

Os exercícios devem ser concluídos em um ambiente de laboratório hospedado. Se você quiser concluí-los em seu próprio computador, você pode fazê-lo instalando o seguinte software. Você pode enfrentar diálogos e comportamento inesperados ao usar seu próprio ambiente. Devido à ampla gama de configurações locais possíveis, a equipe do curso não pode oferecer suporte a problemas que você possa encontrar em seu próprio ambiente.

> **Observação**: As instruções abaixo são para um computador Windows 11. Você também pode usar Linux ou MacOS. Talvez seja necessário adaptar as instruções de laboratório para o sistema operacional escolhido.

### Sistema operacional base (Windows 11)

#### Windows 11

Instale o Windows 11 e aplique todas as atualizações.

#### Edge

Instalar o [Edge (Chromium)](https://microsoft.com/edge)

### SDK do .NET Core

1. Faça download e instale de https://dotnet.microsoft.com/download (faça download do SDK do .NET Core - não apenas o tempo de execução). Caso esteja executando os laboratórios neste curso em seu próprio computador, deverá ter o .NET 7.0. Os laboratórios foram testados no .NET 7.0, mas ele está sem suporte no momento. Você poderá usar o 8.0, mas poderá haver alguns problemas menores. Altamente recomendado usar o ambiente hospedado.

### C++ Redistributable

1. Baixe e instale o Visual C++ Redistributable (x64) de https://aka.ms/vs/16/release/vc_redist.x64.exe.

### Node.JS

1. Baixe a versão mais recente do LTS em https://nodejs.org/en/download/. 
2. Instale usando as opções padrão

### Python (e pacotes necessários)

1. Baixe a versão 3.11 de https://docs.conda.io/en/latest/miniconda.html 
2. Execute a instalação para instalar - **Importante**: Selecione as opções para adicionar Miniconda à variável PATH e registrar Miniconda como o ambiente Python padrão.
3. Após a instalação, abra o prompt do Anaconda e digite os seguintes comandos para instalar pacotes: 

```
pip install flask requests python-dotenv pylint matplotlib pillow
pip install --upgrade numpy
```

### CLI do Azure

1. Faça download em https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest 
2. Instale usando as opções padrão

### Git

1. Baixe e instale de https://git-scm.com/download.html, usando as opções padrão


### Visual Studio Code (e extensões)

1. Faça download em https://code.visualstudio.com/Download 
2. Instale usando as opções padrão 
3. Depois da instalação, inicie o Visual Studio Code e na guia **Extensões** (CTRL+SHIFT+X), procure e instale as seguintes extensões da Microsoft:
    - Python
    - C#
    - Azure Functions
    - PowerShell
