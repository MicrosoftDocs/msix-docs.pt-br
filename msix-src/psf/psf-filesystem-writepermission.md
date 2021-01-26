---
description: Fornece orientação sobre como aplicar correções de permissão de gravação do sistema de arquivos com a estrutura de suporte do pacote.
title: Estrutura de suporte do pacote-correção de permissão de gravação do sistema de arquivos
ms.date: 12/16/2020
ms.topic: article
keywords: Windows 10, UWP, PSF, estrutura de suporte a pacote, FileSystem, permissão de gravação, msix
ms.localizationpriority: medium
ms.openlocfilehash: 577dd4d1f7d68bdf8a7be0bd69732373f97faf36
ms.sourcegitcommit: 059f215a0804adeeefeaaa09b376684caa4382eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98768862"
---
# <a name="package-support-framework---filesystem-write-permission-fixup"></a>Estrutura de suporte do pacote-correção de permissão de gravação do sistema de arquivos

## <a name="investigation"></a>Investigação

Os aplicativos do Windows redirecionarão diretórios específicos relacionados ao aplicativo para a pasta de contêiner do aplicativo do Windows. Se um aplicativo tentar gravar no contêiner do aplicativo do Windows, um erro será disparado e a gravação falhará. 

Usando o PSF (Package support Framework), os aprimoramentos podem ser feitos ao pacote de aplicativos do Windows para resolver esse problema. Primeiro, devemos identificar a falha e os caminhos de diretório que estão sendo solicitados pelo aplicativo.

#### <a name="capture-the-windows-app-failure"></a>Capturar a falha do aplicativo do Windows

Filtrar os resultados é uma etapa opcional, que facilitará a exibição de falhas relacionadas ao aplicativo. Para fazer isso, criaremos duas regras de filtro. O primeiro filtro de inclusão para o nome do processo do aplicativo e o segundo é uma inclusão de todos os resultados que não forem bem-sucedidos.

1. Baixe e extraia o [Monitor de processos do Sysinternals](/sysinternals/downloads/procmon) para o diretório **C:\PSF\ProcessMonitor**
1. Abra o Windows Explorer e navegue até a pasta do monitor de processos do SysInternals extraído
1. Clique duas vezes no arquivo do monitor de processo do SysInternals (procmon.exe), iniciando o aplicativo.
1. Se solicitado pelo UAC, selecione o botão **Sim** .
1. Na janela filtro do Process Monitor, selecione o primeiro menu suspenso rotulado com **arquitetura**.
1. Selecione **nome do processo** no menu suspenso.
1. No próximo menu suspenso, verifique se ele está definido com o valor de **é**.
1. No campo de texto, digite o nome do processo do seu aplicativo (exemplo: PSFSample.exe).
    :::image type="content" source="images/procmon-filterdialog-processname-psfsample.png" alt-text="Exemplo de filtro do processo monitor Windows com o nome do aplicativo":::
1. Selecione o botão **Adicionar**.
1. Na janela filtro do monitor de processos, selecione o primeiro menu suspenso rotulado como **nome do processo**.
1. Selecione **resultado** no menu suspenso.
1. No menu suspenso seguinte, selecione-o e selecione **não está** no menu suspenso.
1. No campo de texto tipo: **êxito**.
    :::image type="content" source="images/procmon-filterdialog-result-success.png" alt-text="Exemplo de janelas de filtro do monitor de processos com resultado":::
1. Selecione o botão **Adicionar**.
1. Selecione o botão **OK** .
1. Inicie o aplicativo do Windows, dispare o erro e feche o aplicativo do Windows.

#### <a name="review-the-windows-app-failure-logs"></a>Examinar os logs de falha de aplicativo do Windows

Depois de capturar os processos de aplicativo do Windows, os resultados precisarão ser investigados para identificar se a falha está relacionada ao diretório de trabalho.

1. Examine os resultados do monitor de processos do SysInternals, procurando falhas descritas na tabela acima.
1. Se os resultados mostrarem um resultado de **"nome não encontrado"** , com os detalhes **"acesso desejado:..."** para seu aplicativo específico direcionado a um diretório fora do **"c:\Arquivos de Files\WindowsApps \\ ... \\ "** (como visto na imagem abaixo), você identificou com êxito uma falha relacionada ao diretório de trabalho, use o artigo [suporte do PSF-sistema de arquivos]() para obter orientação sobre como aplicar a correção do PSF ao seu aplicativo.
:::image type="content" source="images/procmon-psfsampleapp-writefailure.png" alt-text="Exibe a mensagem de erro testemunhada no monitor de processo do SysInternals para falha ao gravar no diretório.":::

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
                "executable": ""
            }
        ],
        "processes": [
            {
                "executable": "",
                "fixups": [
                {
                    "dll": "",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "",
                                    "patterns": [
                                        ""
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
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

1. Defina o `applications.workingdirectory` valor no *config.jsem* para direcionar o caminho da pasta relativa encontrado no campo **Applications.Application.Executable** do arquivo de *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Imagem destacando o local do diretório de trabalho dentro do arquivo AppxManifest.":::

1. Defina o `process.executable` valor no *config.js* como destino o nome do arquivo (sem caminho e extensões) encontrado no campo **Applications.Application.Executable** do arquivo de *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-processexecutable.png" alt-text="Imagem destacando o local do executável do processo dentro do arquivo AppxManifest.":::

1. Defina o `processes.fixups.dll` valor no *config.jsem* para direcionar a arquitetura **FileRedirectionFixup.dll** específica. Se a correção for para a arquitetura x64, defina o valor como **FileRedirectionFixup64.dll**. Se a arquitetura for x86 ou for desconhecida, defina o valor a ser **FileRedirectionFixup86.dll**

1. Defina o `processes.fixups.config.redirectedPaths.packageRelative.base` valor no *config.js* para o caminho da pasta relativa do pacote, conforme encontrado no **Applications.Application.Executable** do arquivo de *AppxManifest.xml* .
    :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Imagem destacando o local do diretório de trabalho dentro do arquivo AppxManifest.":::

1. Defina o `processes.fixups.config.redirectedPaths.packageRelative.patterns` valor no *config.jsno* arquivo para corresponder ao tipo de arquivo que está sendo criado pelo aplicativo. Usando **". * \\ . log"** , o PSF redirecionará as gravações para todos os arquivos de log que estão no diretório identificado no caminho **processes.fixups.config. redirectedPaths. packageRelative. base** , bem como nos diretórios filho.

1. Salve o **config.jsatualizado no** arquivo.
    ```json
    {
        "applications": [
            {
            "id": "PSFSample",
            "executable": "VFS/ProgramFilesX64/PS Sample App/PSFSample.exe"
            }
        ],
        "processes": [
            {
                "executable": "PSFSample",
                "fixups": [
                    {
                        "dll": "FileRedirectionFixup64.dll",
                        "config": {
                            "redirectedPaths": {
                                "packageRelative": [
                                    {
                                        "base": "VFS/ProgramFilesX64/PS Sample App/",
                                        "patterns": [
                                            ".*\\.log"
                                        ]
                                    }
                                ]
                            }
                        }
                    }
                ]
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
    | FileRedirectionFixup64.dll | FileRedirectionFixup64.dll |


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