---
title: Criar um pacote usando a interface de linha de comando
description: Este artigo descreve como criar um pacote MSIX usando a interface de linha de comando para a ferramenta de empacotamento MSIX.
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4ebdb73779821c1ba0dc5fbae844b02df4d204cb
ms.sourcegitcommit: 71c49de79d061909fb1ab632ec7550227d2287bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754826"
---
# <a name="conversion-with-command-line-interface-cli"></a>Conversão com a CLI (interface de linha de comando)

## <a name="automate-conversion-of-windows-installers-to-msix-packages-using-scripts"></a>Automatizar a conversão de instaladores do Windows em pacotes do MSIX usando scripts

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

A Ferramenta de Empacotamento MSIX é compatível com uma interface de linha de comando para a criação de pacotes de aplicativo MSIX. Isso permite que você automatize o processo de reempacotamento de instaladores de aplicativos e execute conversões em massa.

Para os scripts PowerShell e Bash de exemplo que demonstram como automatizar o processo de empacotamento, assinatura, gerenciamento e distribuição de pacotes MSIX, consulte a pasta [scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) do Kit de Ferramentas MSIX.

## <a name="use-the-command-line-interface-cli-with-the-command-prompt"></a>Usar a CLI (interface de linha de comando) com o prompt de comando

Para criar um novo pacote MSIX para seu aplicativo, execute o comando `MsixPackagingTool.exe create-package` em uma janela de prompt de comando do administrador.

Estes são os parâmetros que podem ser passados como argumentos de linha de comando:

|**Parâmetro** |    **Descrição**|
|---------|---------|
|-? --help  |Mostra informações da Ajuda|
|--template | [obrigatório] Caminho para o arquivo XML do modelo de conversão que contém informações do pacote e configurações para essa conversão|
|--virtualMachinePassword   | [opcional] A senha para a Máquina Virtual ser usada para o ambiente de conversão. Observações: o arquivo de modelo deve conter um elemento VirtualMachine e o atributo Settings:: AllowPromptForPassword não deve ser definido como true.|
|-v --verbose   |[opcional] Imprime os logs detalhados no console.|

Exemplos:

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> Atualmente, a conversão do App-V 5.x tem suporte para ser convertida por meio da linha de comando. Isso inclui recursos. 

**Arquivo de modelo de conversão**

``` xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018">

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
    EnforceMicrosoftStoreVersioningRequirements="false">
        
    <!--Note: Exclusion items are optional and if declared take precedence over the default tool exclusion items
        <ExclusionItems>
            <FileExclusion ExcludePath="[{CryptoKeys}]" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Crypto" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Search\Data" />
            <FileExclusion ExcludePath="[{Cookies}]" />
            <FileExclusion ExcludePath="[{History}]" />
            <FileExclusion ExcludePath="[{Cache}]" />
            <FileExclusion ExcludePath="[{Personal}]" />
            <FileExclusion ExcludePath="[{Profile}]\Local Settings" />
            <FileExclusion ExcludePath="[{Profile}]\NTUSER.DAT.LOG1" />
            <FileExclusion ExcludePath="[{Profile}]\ NTUSER.DAT.LOG2" />
            <FileExclusion ExcludePath="[{Recent}]" />
            <FileExclusion ExcludePath="[{Windows}]\debug" />
            <FileExclusion ExcludePath="[{Windows}]\Logs\CBS" />
            <FileExclusion ExcludePath="[{Windows}]\Temp" />
            <FileExclusion ExcludePath="[{Windows}]\WinSxS\ManifestCache" />
            <FileExclusion ExcludePath="[{Windows}]\WindowsUpdate.log" />
        <FileExclusion ExcludePath="[{Windows}]\Installer" />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\$Recycle.Bin " />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\System Volume Information" />
        <FileExclusion ExcludePath="[{AppVPackageDrive}]\Config.Msi" />
            <FileExclusion ExcludePath="[{AppData}]\Microsoft\AppV" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Antimalware" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Windows Defender" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Windows Defender" />
        <FileExclusion ExcludePath="[{ProgramFiles}]\WindowsApps" />
            <FileExclusion ExcludePath="[{Local AppData}]\Temp" />
        <FileExclusion ExcludePath="[{Local AppData}]\Microsoft\Windows" />
        <FileExclusion ExcludePath="[{Local AppData}]\Packages" />

            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware Setup" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Security Client" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\AppV" />
        </ExclusionItems>
    -->
        
    </Settings>

    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute and is optional
    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService ="true"/>
    -->

    <SaveLocation
    PackagePath="C:\users\user\Desktop\MyPackage.msix" 
    TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />
    
    
    <!--NOTE: This section specifies that the conversion will be run on a local Virtual Machine.
    <VirtualMachine Name="vmname" Username="vmusername" />
    -->

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">
        
    <!--NOTE: This ID will be used if the Application entry detected matches the specified ExecutableName
        <Applications>
            <Application
                Id="MyApp1"
                Description="MyApp"
                DisplayName="My App"
                ExecutableName="MyApp.exe"/>
        </Applications>
    -->

    <!--NOTE: This is optional as “runFullTrust” capability is added by default during conversion
        <Capabilities>
            <Capability Name="runFullTrust" />
        </Capabilities>
    -->
        
    </PackageInformation>
</MsixPackagingToolTemplate>
```

**Referência de parâmetros do modelo de conversão**

Esta é a lista completa de parâmetros que você pode usar no arquivo de modelo de conversão.

|**ConversionSettings** |   **Descrição** |
|---------|---------|
|Configurações:: AllowTelemetry |        [opcional] Habilita o log de telemetria para essa invocação da ferramenta.|
|Configurações:: ApplyAllPrepareComputerFixes     |  [opcional] Aplica todas as correções de preparação do computador recomendadas. Não pode ser definida quando outros atributos são usados.|
|Configurações:: GenerateCommandLineFile  |  [opcional] Copia a entrada do arquivo de modelo para o diretório SaveLocation para uso futuro.|
|Configurações:: AllowPromptForPassword |        [opcional] Instrui a ferramenta a solicitar que o usuário insira senhas para a Máquina Virtual e o certificado de autenticação, caso ele seja necessário e não especificado.|
|Configurações:: EnforceMicrosoftStoreVersioningRequirements |       [opcional] Instrui a ferramenta a impor o esquema de controle de versão do pacote necessário para a implantação por meio da Microsoft Store e da Microsoft Store para Empresas.|
|ExclusionItems |       [opcional] 0 ou mais elementos FileExclusion ou RegistryExclusion. Todos os elementos FileExclusion precisam aparecer antes dos elementos RegistryExclusion.|
|ExclusionItems::FileExclusion |        [opcional] Um arquivo a ser excluído para o empacotamento.|
|ExclusionItems::FileExclusion::ExcludePath |       Caminho do arquivo a ser excluído para o empacotamento.|
|ExclusionItems::RegistryExclusion |        [opcional] Uma chave do Registro a ser excluído para o empacotamento.|
|ExclusionItems::RegistryExclusion:: ExcludePath |      Caminho do Registro a ser excluído para o empacotamento.|
|PrepareComputer::DisableDefragService |        [opcional] Desabilita o Desfragmentador do Windows enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsSearchService |        [opcional] Desabilita o Windows Search enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableSmsHostService     |  [opcional] Desabilita o Host do SMS enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsUpdateService   |  [opcional] Desabilita o Windows Update enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|SaveLocation     |[opcional] Um elemento para especificar a localização de salvamento da ferramenta. Se não for especificado, o pacote será salvo na pasta Área de Trabalho.         |
|SaveLocation::PackagePath     |[opcional] O caminho para o arquivo ou a pasta em que o pacote MSIX resultante é salvo.         |
|SaveLocation::TemplatePath    |[opcional] O caminho para o arquivo ou a pasta em que o modelo da CLI resultante é salvo.    |
|Installer::Path |      O caminho para o Instalador de Aplicativo.|
|Installer::Arguments |     [opcional] Os argumentos a serem passados para o instalador.  A ferramenta executará de forma automática os instaladores MSI silenciosamente usando o argumento "/qn /norestart INSTALLSTARTMENUSHORTCUTS=1 DISABLEADVTSHORTCUTS=1". Observação: você deve passar os argumentos para forçar o instalador a executar silenciosamente se você estiver usando instaladores. exe.|
|Installer::InstallLocation |       adicional O caminho completo para a pasta raiz do aplicativo para os arquivos instalados, se ele foi instalado (por exemplo, "C:\Arquivos de programas (x86) \MyAppInstalllocation").|
|Computador Virtual |       [opcional] Um elemento para especificar que a conversão será executada em uma Máquina Virtual local.|
|VrtualMachine::Name     |  O nome da Máquina Virtual a ser usado para o ambiente de conversão.|
|VirtualMachine::Username |     [opcional] O nome de usuário da Máquina Virtual a ser usado para o ambiente de conversão.|
|PackageInformation::PackageName |      O nome do pacote MSIX.|
|PackageInformation::PackageDisplayName |       O nome de exibição do pacote MSIX.|
|PackageInformation::PublisherName |        O fornecedor do pacote MSIX.|
|PackageInformation::PublisherDisplayName |     O nome de exibição do fornecedor do pacote MSIX.|
|PackageInformation::Version |      O número de versão do pacote MSIX.|
|PackageInformation:: MainPackageNameForModificationPackage |       [opcional] O nome da identidade do pacote do nome do pacote principal. Isso é usado durante a criação de um pacote de modificação que tem uma dependência de um aplicativo principal (pai).|
|Aplicativo |     [opcional] 0 ou mais elementos Application para configurar as entradas de Application no pacote MSIX.|
|Application::Id |      A ID do Aplicativo MSIX. Essa ID será usada para a entrada Application detectada que corresponde ao ExecutableName especificado. Você pode ter vários valores da ID do Aplicativo para executáveis no pacote.<br/><br/>Esse valor é o identificador exclusivo do aplicativo no pacote. Às vezes, esse valor é denominado PRAID (identificador do aplicativo relativo do pacote). A ID precisa ser exclusiva no pacote (a mesma ID não pode ser usada mais de uma vez no mesmo pacote). No entanto, a ID não precisa ser globalmente exclusiva. Pode haver outro pacote no sistema que usa a mesma ID.<br/><br/>Essa cadeia de caracteres contém campos alfanuméricos separados por pontos. Cada campo precisa começar com um caractere alfabético ASCII. Você não pode usá-los como valores de campo: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8" e "LPT9".|
|Application::ExecutableName |      O nome do executável do aplicativo MSIX que será adicionado ao manifesto do pacote. A entrada de aplicativo correspondente será ignorada se nenhum aplicativo com esse nome for detectado.|
|Application::Description |     [opcional] A descrição do aplicativo MSIX. Se não for usado, o Application DisplayName será usado. Essa descrição será usada para a entrada do aplicativo detectada que corresponde ao ExecutableName especificado|
|Application::DisplayName    |  O nome de exibição do aplicativo para o pacote MSIX. Esse nome de exibição será usado para a entrada do aplicativo detectada que corresponde ao ExecutableName especificado|
|Capacidades |     [opcional] 0 ou mais elementos Capability para adicionar funcionalidades personalizadas ao pacote MSIX. A funcionalidade “runFullTrust” é adicionada por padrão durante a conversão.|
|Capability::Name | A capacidade a ser adicionada ao pacote MSIX.

