---
title: Criando o pacote MSIX com o MSIX Core do código-fonte
description: Este artigo fornece instruções passo a passo sobre como aproveitar o bootstrapper MSIX Core, que cria um aplicativo usando o ClickOnce que permitirá que os usuários baixem apenas um setup. exe e instalem seu aplicativo MSIX por meio do instalador do MSIX Core.
ms.date: 11/15/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: b98a26ce313b0c828fbad788541ed42a975f40fc
ms.sourcegitcommit: 8aafaa9ac4087ef2e95030343add8fe2ee1cccc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/18/2019
ms.locfileid: "75186802"
---
# <a name="creating-msix-package-with-msix-core-from-source-code"></a>Criando o pacote MSIX com o MSIX Core do código-fonte 

Se você tiver aplicativos de área de trabalho existentes e quiser dar suporte a seus clientes que usam o Windows 7 SP1, Windows 8.1, atualmente com suporte no Windows Server (com experiência desktop) e versões do Windows 10 anteriores à 1709 (atualização de aniversário de outono), você pode aproveitar o MSIX Instalador principal para criar um aplicativo usando o ClickOnce. Isso permitirá que os usuários baixem um setup. exe e instalem o aplicativo MSIX por meio do instalador do MSIX Core.

## <a name="host-your-app-on-a-web-server"></a>Hospedar seu aplicativo em um servidor Web

Para preparar seu aplicativo para inicialização com o instalador do MSIX Core, você precisará hospedar o pacote do aplicativo em um servidor Web. Esta seção fornece detalhes sobre como configurar um aplicativo Web no [Azure](#azure), [serviços de informações da Internet (IIS)](#internet-information-services-iis)e [Amazon Web Services (AWS)](#amazon-web-services-aws).

### <a name="azure"></a>Azure

Para usar essa opção, você deve ter uma assinatura do Azure. Para obter uma, consulte a [página da conta do Azure](https://azure.microsoft.com/free/).

#### <a name="create-an-azure-web-app"></a>Criar um aplicativo Web do Azure

Para começar, vá para a [página de portal do Azure](https://portal.azure.com/) e siga estas etapas:

1. Clique em **criar um recurso**.
2. Clique em **Web** e selecione **aplicativo Web**.
3. Em **detalhes da instância**, crie um nome de aplicativo exclusivo e selecione as configurações apropriadas para seu aplicativo. Por exemplo, você precisará escolher entre o **código ou o contêiner do Docker** e a **pilha de tempo de execução**. Caso contrário, deixe todos os outros padrões.
4. Clique em **criar** e conclua o assistente.

#### <a name="host-the-app-package-and-the-web-page"></a>Hospedar o pacote do aplicativo e a página da Web

1. Depois de criar o aplicativo Web, selecione o aplicativo.
2. Em **ferramentas de desenvolvimento**, clique em **Editor do serviço de aplicativo**.
3. No editor, há um arquivo **hostingstart. html** padrão. Clique com o botão direito do mouse no espaço vazio do explorador de arquivos e selecione **carregar arquivos** para começar a carregar seus pacotes de aplicativos.
4. Clique com o botão direito do mouse no espaço vazio do painel explorador de arquivos novamente e selecione **novos arquivos** para criar um novo arquivo. Nomeie o arquivo como você deseja que sua página HTML padrão seja.

#### <a name="configure-the-web-app-for-app-package-mime-types"></a>Configurar o aplicativo Web para tipos de MIME do pacote de aplicativos

Adicione um novo arquivo chamado Web. config ao aplicativo Web. Abra o arquivo Web. config e adicione o seguinte XML ao arquivo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extensions-->
        <staticContent>
            <mimeMap fileExtension=".appx" mimeType="application/appx" />
            <mimeMap fileExtension=".msix" mimeType="application/msix" />
        </staticContent>
    
    </system.webServer>
</configuration>
```

### <a name="internet-information-services-iis"></a>Serviços de Informações da Internet (IIS)

O IIS é um recurso opcional do Windows. Para instalar o IIS:

1. Clique em **Iniciar** e procure **Ativar ou desativar recursos do Windows**.
2. Selecione **serviços de informações da Internet**. 
3. Além disso, certifique-se de instalar o **ASP.NET 4,5** ou superior. Na caixa de diálogo **recursos do Windows** , expanda **serviços de informações da Internet** -> **World Wide Web serviços** -> **recursos de desenvolvimento de aplicativos**e selecione uma versão de **ASP.net** que seja maior ou igual a **ASP.NET 4,5**.
4. Clique em **OK** para iniciar a instalação.

O Visual Studio 2017 (ou uma versão posterior) e as ferramentas de desenvolvimento para a Web são necessários. Se você já tiver o Visual Studio 2017 ou uma versão posterior instalada, verifique se você tem as cargas de trabalho de desenvolvimento de ASP.NET e Web instaladas. Caso contrário, instale o Visual Studio [aqui](https://docs.microsoft.com/visualstudio/install/install-visual-studio).

#### <a name="build-a-web-app"></a>Crie um aplicativo Web

Inicie o Visual Studio como administrador e crie um novo projeto de **aplicativo Web Visual C#**  com um modelo de projeto vazio.

#### <a name="configure-iis-with-your-web-app"></a>Configurar o IIS com seu aplicativo Web

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto raiz e selecione **Propriedades**. 
2. Em Propriedades, selecione a guia **Web** . 
3. Na seção **servidores** , escolha **local IIS** no menu suspenso e clique em **criar diretório virtual**.

#### <a name="add-the-app-package-to-the-web-application"></a>Adicionar o pacote do aplicativo ao aplicativo Web

Adicione o pacote do aplicativo que você deseja distribuir para o aplicativo Web:

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó do projeto.
2. Selecione **adicionar** -> **nova pasta** e nomeie a pasta **pacotes**. 
3. Para adicionar pacotes de aplicativos à pasta, clique com o botão direito do mouse na pasta pacotes e selecione **adicionar** -> **Item existente**. Navegue até o local do pacote do aplicativo.

#### <a name="create-a-web-page"></a>Criar uma página da Web

Crie uma página HTML ou qualquer outro aplicativo Web conforme necessário de acordo com suas necessidades. Adicione o link do seu novo setup. exe.

#### <a name="configure-the-web-app-for-app-package-mime-types"></a>Configurar o aplicativo Web para tipos de MIME do pacote de aplicativos

Abra o arquivo **Web. config** no Gerenciador de soluções e adicione o seguinte XML no elemento <configuration>.

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extensions-->
    <staticContent>
        <mimeMap fileExtension=".appx" mimeType="application/appx" />
        <mimeMap fileExtension=".msix" mimeType="application/msix" />
    </staticContent>
</system.webServer>
```

### <a name="amazon-web-services-aws"></a>Amazon Web Services (AWS)

Para usar essa opção, você deve ter uma associação AWS. Para obter mais informações, consulte [detalhes da conta do AWS](https://aws.amazon.com/free/).

#### <a name="create-an-amazon-s3-bucket-and-upload-your-msix-packages-and-web-pages"></a>Criar um Bucket do Amazon S3 e carregar seus pacotes MSIX e páginas da Web

O Amazon S3 (Simple Storage Service) é uma oferta AWS para coletar, armazenar e analisar dados. Os buckets S3 são uma maneira conveniente de hospedar pacotes de aplicativos do Windows 10 e páginas da Web para distribuição.

1. Faça logon no AWS. Em **Serviços** , localize **S3**.
2. Selecione **criar Bucket** e insira um **nome de Bucket** para seu site. Siga os prompts de diálogo para definir as propriedades e as permissões. Para garantir que seu aplicativo do Windows 10 possa ser distribuído do seu site, habilite as permissões de **leitura** e **gravação** para o Bucket e selecione **conceder acesso de leitura público a este Bucket**. Clique em **criar Bucket** para concluir esta etapa.
3. Quando tiver terminado, carregue seus pacotes MSIX e páginas da Web para o Bucket S3.

#### <a name="configure-the-web-app-for-app-package-mime-types"></a>Configurar o aplicativo Web para tipos de MIME do pacote de aplicativos
Usar uma interface de serviço da Web como de [navegador S3](https://s3browser.com/features-content-mime-types-editor.aspx) para adicionar novos **cabeçalhos HTTP padrão**. 
1. Navegue até **ferramentas** e selecione **cabeçalhos HTTP padrão**.
2. Na caixa de diálogo **cabeçalhos HTTP padrão** , clique em **Adicionar**.
3. Na caixa de diálogo **Adicionar novos cabeçalhos HTTP padrão** , especifique o nome do Bucket, o nome do arquivo, o nome do cabeçalho e o valor do cabeçalho e, em seguida, clique em **Adicionar novo cabeçalho**.
    * **Nome do Bucket**: msix-Packages
    * **Nome do arquivo**: *. msix
    * **Nome do cabeçalho**: Content-Type
    * **Valor do cabeçalho**: Application/msix

> [!NOTE]
> AWS têm algumas diretrizes estritas que você precisará seguir. Por exemplo, os nomes de buckets devem ser exclusivos e, portanto, se você estiver usando o exemplo acima, será necessário alterar o nome do Bucket. 

## <a name="use-the-msix-core-installer-to-build-the-clickonce-application"></a>Usar o instalador do MSIX Core para compilar o aplicativo ClickOnce

Localize o aplicativo de aplicativo ClickOnce setup. exe. 
### <a name="run-url-command-to-create-new-setupexe"></a>Execute o comando URL para criar um novo setup. exe
#### <a name="prerequisite"></a>Pré-requisito 
Verifique se você seguiu as instruções para clonar, compilar e publicar a solução MSIX Core no Visual estúdios. 

Navegue até o diretório em que você baixou o arquivo setup. exe e execute este comando: 
```
setup-exe - url=<location of your msix in the webservice>
```

### <a name="sign-the-application"></a>Assinar o aplicativo

Como a etapa anterior criou um novo setup. exe, será necessário assinar o aplicativo novamente para verificar se você é um editor confiável do aplicativo e estabelecer a integridade do aplicativo. Você pode usar o [SignTool](https://docs.microsoft.com/dotnet/framework/tools/signtool-exe) e fornecer seu certificado.

### <a name="distribute-the-application-to-your-users"></a>Distribua o aplicativo para seus usuários

Agora você pode apontar para o novo setup. exe com um link ou botão de download em seu site. O MSIX Core destina-se a usuários no Windows 10, versão 1703 e anteriores. O [instalador do aplicativo](https://docs.microsoft.com/windows/msix/app-installer/installing-windows10-apps-web) é o processo de instalação ideal para pacotes MSIX no Windows 1709 ou uma versão posterior. O instalador do aplicativo otimiza o espaço em disco no lado do consumidor e pode instalar aplicativos diretamente de locais HTTP. O MSIX Core detectará se um consumidor está no Windows 1709 ou em uma versão posterior e os redirecionará para o instalador do aplicativo. 


No Microsoft Edge, você pode chamar o método [getHostEnvironmentValue ()](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/mt795399%28v%3dvs.85%29) e o campo *so-Build* no valor de retorno especificará a versão do sistema operacional do usuário. A partir daí, você pode solicitar que o processo de instalação use o MSIX Core (para Windows 10, versão 1703 e anterior) ou o instalador do aplicativo (para Windows 10, versão 1709 e posterior).

## <a name="user-experience"></a>Experiência do usuário

Os usuários simplesmente baixam e executam o setup. exe na página da Web do desenvolvedor.

* Se o instalador do MSIX Core ainda não estiver instalado quando o usuário executar Setup. exe, o usuário verá o prompt do ClickOnce e clicará em **instalar** para instalar o instalador do MSIX Core. O instalador é iniciado automaticamente e mostra a tela de instalação do pacote MSIX especificado na cadeia de caracteres de consulta do desenvolvedor para que os usuários possam instalar o aplicativo.
* Se o instalador do MSIX Core já estiver instalado quando o usuário executar Setup. exe, o instalador do MSIX Core será iniciado automaticamente e mostrará a tela de instalação do pacote MSIX especificado na cadeia de caracteres de consulta para que os usuários instalem o aplicativo.
