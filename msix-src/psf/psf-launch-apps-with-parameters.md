---
description: Fornece orientação sobre como aplicar correções de tempo de execução com a estrutura de suporte do pacote.
title: Estrutura de suporte de pacote-iniciando aplicativos do Windows com parâmetros
ms.date: 12/16/2020
ms.topic: article
keywords: Windows 10, UWP, PSF, estrutura de suporte a pacotes, argumentos, inicializador de aplicativos, parâmetros, atalho, msix
ms.localizationpriority: medium
ms.openlocfilehash: 4eaee3a4b514527500f715ec3390fbad136790c7
ms.sourcegitcommit: 059f215a0804adeeefeaaa09b376684caa4382eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98768853"
---
# <a name="launching-windows-app-with-parameters"></a>Iniciando o aplicativo do Windows com parâmetros

## <a name="investigation"></a>Investigação

Alguns iniciadores de aplicativos do Windows no menu iniciar exigem o uso de parâmetros a serem passados para o executável ao iniciar o aplicativo do Windows. Para fazer isso, primeiro precisaremos identificar o iniciador que requer o parâmetro antes de integrar o aplicativo do Windows com a estrutura de suporte do pacote.

#### <a name="identifying-windows-app-launcher-parameter-requirement"></a>Identificando requisito de parâmetro do iniciador do aplicativo Windows

1. Instale seu aplicativo do Windows em um computador de teste.
1. Abra o **menu Iniciar do Windows**.
1. Localize e selecione o inicializador de aplicativos do Windows no menu iniciar.
1. Se o aplicativo for iniciado, você não terá nenhum problema (teste todos os iniciadores de aplicativo do Windows associados no menu Iniciar).
1. Desinstale o aplicativo do Windows no computador de teste.
1. Usando a mídia de instalação do Win32, instale o aplicativo em seu computador de teste.
1. Abra o **menu Iniciar do Windows**.
1. Localize e clique com o botão direito do mouse no seu aplicativo do Windows de dentro do menu iniciar.
1. Selecione **mais**  >>  **local do arquivo aberto** no menu suspenso.
1. Clique com o botão direito do mouse no primeiro atalho do aplicativo associado (repita as três próximas etapas para todos os atalhos de aplicativo associados).
1. Selecione **Propriedades** no menu suspenso.
1. Examine o valor na caixa de texto à direita de **destino**. Após o caminho do arquivo de aplicativo, se houver um parâmetro listado, este :::image type="content" source="images/contosoapp-fileproperties-target-parameter.png" alt-text="exemplo de aplicativo da janela de propriedades de arquivo com o parâmetro no destino":::

1. Registre o valor do parâmetro para uso futuro.


## <a name="resolution"></a>Resolução

Os aplicativos do Windows redirecionarão diretórios específicos relacionados ao aplicativo para a pasta de contêiner do aplicativo do Windows. Se um aplicativo tentar gravar no contêiner do aplicativo do Windows, um erro será disparado e a gravação falhará. 

Para resolver o problema relacionado à falha do aplicativo do Windows em gravar no contêiner de aplicativos do Windows, é necessário seguir as quatro etapas a seguir:

1. [Preparar o aplicativo do Windows para um diretório local](#stage-the-windows-app)
1. [Criar o Config.jse injetar os arquivos PSF necessários](#create-and-inject-required-psf-files)
1. [Atualizar o arquivo AppxManifest do aplicativo do Windows](#update-appxmanifest)
1. [Reempacotar e assinar o aplicativo do Windows](#re-package-the-application)

As etapas acima fornecem orientação por meio da extração do conteúdo do aplicativo do Windows para um diretório de preparo local, injetando os arquivos de correção PSF no diretório de aplicativo do Windows preparado, configurando o iniciador do aplicativo para apontar para o iniciador do PSF e, em seguida, configurar o PSF config.jsno arquivo para redirecionar o iniciador PSF para o aplicativo que especifica o diretório

### <a name="download-and-install-required-tools"></a>Baixar e instalar as ferramentas necessárias
Esse processo orientará você durante a recuperação de e o uso das seguintes ferramentas:
- Ferramenta de cliente NuGet
- PSF (estrutura de suporte do pacote)
- SDK do Windows 10 (versão mais recente)
- Monitor de processos da SysInternals

O seguinte fornecerá orientações passo a passo sobre como baixar e instalar as ferramentas necessárias.

1. Baixe a versão mais recente (sem visualização) da [ferramenta de cliente do NuGet](https://www.nuget.org/downloads)e salve o **nuget.exe** na `C:\PSF\nuget` pasta.
1. Baixe a estrutura de suporte do pacote usando o [NuGet](/nuget/install-nuget-client-tools#nugetexe-cli) executando o seguinte em uma janela administrativa do PowerShell:
    ```PowerShell
    Set-Location "C:\PSF"
    .\nuget\nuget.exe install Microsoft.PackageSupportFramework
    ``` 
    
1. Baixe e instale o [Kit de ferramentas de desenvolvimento de software do Windows 10 (SDK do Win 10)](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).
    1. Baixe o [SDK do Win 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).
    1. Execute o **winsdksetup.exe** que foi baixado na etapa anterior.
    1. Selecione o botão **Avançar**.
    1. Selecione apenas os três recursos a seguir para instalação:
        - Ferramentas de assinatura SDK do Windows para aplicativos da área de trabalho
        - SDK do Windows para aplicativos UWP C++
        - DoWindows SDK para localização de aplicativos UWP
    1. Selecione o botão **Instalar**.
    1. Selecione o botão **OK** .

### <a name="stage-the-windows-app"></a>Preparar o aplicativo do Windows
Ao preparar o aplicativo do Windows, iremos extrair/desempacotar o conteúdo do aplicativo do Windows para um diretório local. Depois que o aplicativo do Windows for desempacotado no local de preparo, os arquivos de correção PSF poderão ser injetados corrigindo quaisquer experiências indesejadas.

1. Abra uma janela administrativa do PowerShell.
1. Defina as seguintes variáveis direcionando seu arquivo de aplicativo específico e a versão do SDK do Windows 10:
    ```PowerShell
    $AppPath          = "C:\PSF\SourceApp\PSFSampleApp.msix"         ## Path to the MSIX App Installer
    $StagingFolder    = "C:\PSF\Staging\PSFSampleApp"                ## Path to where the MSIX App will be staged
    $OSArchitecture   = "x$((gwmi Win32_Processor).AddressWidth)"    ## Operating System Architecture
    $Win10SDKVersion  = "10.0.19041.0"                               ## Latest version of the Win10 SDK
    ```

1. Descompacte o aplicativo do Windows para a pasta de preparo executando o seguinte cmdlet do PowerShell:
    ```PowerShell
    ## Sets the directory to the Windows 10 SDK
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    
    ## Unpackages the Windows App to the staging folder
    .\makeappx.exe unpack /p "$AppPath" /d "$StagingFolder"
    ```


### <a name="create-and-inject-required-psf-files"></a>Criar e injetar arquivos PSF necessários
Para aplicar ações corretivas ao aplicativo do Windows, é necessário criar um **config.jsno** arquivo e fornecer informações sobre o inicializador de aplicativos do Windows que está falhando. Se houver vários iniciadores de aplicativos do Windows que estão enfrentando problemas, o **config.jsno** arquivo poderá ser atualizado com várias entradas.

Depois de atualizar o **config.jsno** arquivo, o **config.jsno** arquivo e o suporte aos arquivos de correção PSF devem ser movidos para a raiz do pacote de aplicativos do Windows. 


1. Abra Visual Studio Code (VS Code) ou qualquer outro editor de texto.
1. Crie um novo arquivo, selecionando o menu **arquivo** na parte superior da vs Code, selecionando **novo arquivo** no menu suspenso.
1. Salve o arquivo como **config.jsem**, selecionando o menu **arquivo** na parte superior da janela vs Code, selecionando **salvar** no menu suspenso. Na janela salvar como, navegue até o diretório de preparo do aplicativo do Windows (**C:\PSF\Staging\PSFSampleApp**) e defina o **nome do arquivo** como `config.json` . Selecione o botão **Salvar**.
1. Copie o código a seguir para a **config.jsrecém-criada no** arquivo.
    ```JSON
    {
        "applications": [
            {
                "id": "",
                "executable": "",
                "arguments": ""
            }
        ]
    }
    ```

1. Abra o arquivo **AppxManifest** de aplicativo do Windows em etapas localizado na pasta de preparo do aplicativo do windows (**C:\PSF\Staging\PSFSampleApp\AppxManifest.xml**) usando vs Code ou outro editor de texto.
    ```xml
    <Applications>
        <Application Id="PSFSAMPLE" Executable="VFS\ProgramFilesX64\PS Sample App\PSFSample.exe" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements BackgroundColor="transparent" DisplayName="PSFSample" Square150x150Logo="Assets\StoreLogo.png" Square44x44Logo="Assets\StoreLogo.png" Description="PSFSample">
            <uap:DefaultTile Wide310x150Logo="Assets\StoreLogo.png" Square310x310Logo="Assets\StoreLogo.png" Square71x71Logo="Assets\StoreLogo.png" />
        </uap:VisualElements>
        </Application>
    </Applications>
    ```

1. Defina o `applications.id` valor no *config.js* para que seja o mesmo valor que foi encontrado no campo **Applications.Application.ID** do arquivo *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-id.png" alt-text="Imagem destacando o local da ID no arquivo AppxManifest.":::

1. Defina o `applications.executable` valor no *config.jsem* para direcionar o caminho relativo para o aplicativo localizado no campo **Applications.Application.Executable** do arquivo de *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-executable.png" alt-text="Imagem destacando o local do executável no arquivo AppxManifest.":::

1. Defina o `applications.arguments` valor no *config.js* para corresponder ao argumento usado para iniciar o aplicativo. Consulte o valor gravado na etapa final do guia de [investigação-identificando as diretrizes de requisito de parâmetro do inicializador de aplicativo do Windows](#identifying-windows-app-launcher-parameter-requirement) .

1. Defina o `applications.workingdirectory` valor no *config.jsem* para direcionar o caminho da pasta relativa encontrado no campo **Applications.Application.Executable** do arquivo de *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Imagem destacando o local do diretório de trabalho dentro do arquivo AppxManifest.":::

1. Salve o **config.jsatualizado no** arquivo.
    ```json
    {
        "applications": [
            {
            "id": "PSFSample",
            "executable": "VFS/ProgramFilesX64/PS Sample App/PSFSample.exe",
            "arguments": "/bootfromsettingshortcut"
            }
        ]
    }
    ```

1. Copie os quatro arquivos a seguir da estrutura de suporte do pacote com base na arquitetura executável do aplicativo para a raiz do aplicativo do Windows em etapas. Os seguintes arquivos estão localizados dentro do **.\Microsoft.PackageSupportFramework. <Version> \bin**.

    | Aplicativo (x64)          | Aplicativo (x86)          | 
    |----------------------------|----------------------------|
    | PSFLauncher64.exe          | PSFLauncher32.exe          |
    | PSFRuntime64.dll           | PSFRuntime32.dll           |
    | PSFRunDll64.exe            | PSFRunDll32.exe            |


### <a name="update-appxmanifest"></a>Atualizar AppxManifest
Depois de criar e atualizar o **config.jsno** arquivo, o **AppxManifest.xml** do aplicativo do Windows deve ser atualizado para cada inicializador de aplicativos do Windows que foi incluído no **config.jsem**. Os aplicativos da **AppxManifest** agora devem ter como destino o **PSFLauncher.exe** associado à arquitetura de aplicativos.


1. Abra o explorador de arquivos e navegue até a pasta MSIX do aplicativo de preparo (**C:\PSF\Staging\PSFSampleApp**).
1. Clique com o botão direito do mouse em **AppxManifest.xml** e selecione **abrir com código** no menu suspenso (opcionalmente, você pode abrir com outro editor de texto).

1. Atualize o arquivo de AppxManifest.xml com as seguintes informações:
    ```xml
    <Package ...>
    ...
    <Applications>
        <Application Id="PSFSample"
                    Executable="PSFLauncher32.exe"
                    EntryPoint="Windows.FullTrustApplication">
        ...
        </Application>
    </Applications>
    </Package>
    ```


### <a name="re-package-the-application"></a>Reempacotar o aplicativo
Todas as correções foram aplicadas, agora o aplicativo do Windows pode ser empacotado novamente em um MSIX e assinado usando um certificado de assinatura de código.

1. Abra uma janela administrativa do PowerShell.
1. Defina as seguintes variáveis:
    ```PowerShell
    $AppPath          = "C:\PSF\SourceApp\PSFSampleApp_Updated.msix" ## Path to the MSIX App Installer
    $CodeSigningCert  = "C:\PSF\Cert\CodeSigningCertificate.pfx"     ## Path to your code signing certificate
    $CodeSigningPass  = "<Password>"                                 ## Password used by the code signing certificate
    $StagingFolder    = "C:\PSF\Staging\PSFSampleApp"                ## Path to where the MSIX App will be staged
    $OSArchitecture   = "x$((gwmi Win32_Processor).AddressWidth)"    ## Operating System Architecture
    $Win10SDKVersion  = "10.0.19041.0"                               ## Latest version of the Win10 SDK
    ```

1. Novamente o aplicativo do Windows da pasta de preparo executando o seguinte cmdlet do PowerShell:
    ```PowerShell
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    .\makeappx.exe pack /p "$AppPath" /d "$StagingFolder"
    ```

1. Assine o aplicativo do Windows executando o seguinte cmdlet do PowerShell:
    ```PowerShell
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    .\signtool.exe sign /v /fd sha256 /f $CodeSigningCert /p $CodeSigningPass $AppPath
    ```