---
title: Criar um pacote usando a interface de linha de comando
description: Saiba como criar um pacote MSIX usando a interface de linha de comando.
author: mcleanbyron
ms.author: mcleans
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 538855e6d32beaac571253b7d880db9b21897174
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900888"
---
# <a name="conversion-with-cli"></a>Conversão com a CLI

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>
      
Para criar um novo pacote MSIX para seu aplicativo, execute o comando Criar pacote de MsixPackagingTool.exe em uma janela de prompt de comando do administrador. 

Aqui estão os parâmetros que podem ser passados como argumentos de linha de comando:

|**Parâmetro** |    **Descrição**|
|---------|---------|
|-? --help  |Mostrar informações de ajuda|
|-modelo | [obrigatório] caminho para o arquivo XML de modelo de conversão que contém informações de pacote e as configurações para essa conversão|
|--virtualMachinePassword   | [opcional] A senha para a máquina Virtual a ser usado para o ambiente de conversão. Observações: O arquivo de modelo deve conter um elemento de máquina virtual e o atributo Settings::AllowPromptForPassword não deve ser definido como true.|
|-v – detalhado   |[opcional] Logs detalhados para o console de impressão.|

Exemplos:

' ' prompt de comando

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

**Conversion template file**

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

**Referência de parâmetro de modelo de conversão**

Aqui está a lista completa de parâmetros que você pode usar no arquivo de modelo de conversão.

|**ConversionSettings** |   **Descrição** |
|---------|---------|
|Configurações:: AllowTelemetry |        [opcional] Habilita o log de telemetria para essa invocação da ferramenta.|
|Configurações:: ApplyAllPrepareComputerFixes     |  [opcional] Aplica-se todos recomendável preparar computador correções. Não é possível configurar outros atributos são usados.|
|Configurações:: GenerateCommandLineFile  |  [opcional] Copia a entrada do arquivo de modelo para o diretório SaveLocation para uso futuro.|
|Configurações:: AllowPromptForPassword |        [opcional] Instrui a ferramenta para solicitar que o usuário digitar senhas para a máquina Virtual e o certificado de autenticação, se for necessário e não especificado.|
|Configurações:: EnforceMicrosoftStoreVersioningRequirements |       [opcional] Instrui a ferramenta para impor o esquema de controle de versão do pacote necessário para a implantação da Microsoft Store e Microsoft Store para empresas.|
|ExclusionItems |       [opcional] 0 ou mais elementos FileExclusion ou RegistryExclusion. Todos os elementos de FileExclusion devem aparecer antes de quaisquer elementos RegistryExclusion.|
|ExclusionItems::FileExclusion |        [opcional] Um arquivo a ser excluído para o empacotamento.|
|ExclusionItems::FileExclusion::ExcludePath |       Caminho do arquivo a ser excluído para o empacotamento.|
|ExclusionItems::RegistryExclusion |        [opcional] Uma chave do registro a ser excluído para o empacotamento.|
|ExclusionItems::RegistryExclusion:: ExcludePath |      Caminho do registro a ser excluído para o empacotamento.|
|PrepareComputer::DisableDefragService |        [opcional] Desabilita o Desfragmentador do Windows enquanto o aplicativo está sendo convertido. Se definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsSearchService |        [opcional] Desabilita o Windows Search enquanto o aplicativo está sendo convertido. Se definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableSmsHostService     |  [opcional] Desabilita o Host do SMS enquanto o aplicativo está sendo convertido. Se definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsUpdateService   |  [opcional] Desabilita a atualização do Windows enquanto o aplicativo está sendo convertido. Se definido como false, substituirá ApplyAllPrepareComputerFixes.|
|SaveLocation     |[opcional] Um elemento para especificar o salvamento local da ferramenta. Se não for especificado, o pacote será salvo na pasta da área de trabalho.         |
|SaveLocation::PackagePath     |[opcional] O caminho para o arquivo ou pasta em que o pacote MSIX resultante é salvo.         |
|SaveLocation::TemplatePath    |[opcional] O caminho para o arquivo ou pasta em que o modelo resultante da CLI é salvo.    |
|Installer::path |      O caminho para o instalador do aplicativo.|
|Installer::arguments |     [opcional] Os argumentos para passar para o instalador.  A ferramenta será executada automaticamente os instaladores MSI silenciosamente usando o argumento "/qn /norestart INSTALLSTARTMENUSHORTCUTS = 1 DISABLEADVTSHORTCUTS = 1". OBSERVAÇÃO: Você deve passar os argumentos para forçar o instalador seja executado silenciosamente, se você estiver usando .exe instaladores.|
|Installer::InstallLocation |       [opcional] O caminho completo para a pasta do aplicativo raiz para os arquivos instalados se estivesse instalado (por exemplo "Arquivos (x86) de C:\Program \MyAppInstalllocation").|
|Computador Virtual |       [opcional] Um elemento para especificar que a conversão será executada em uma máquina Virtual local.|
|VrtualMachine::Name     |  O nome da máquina Virtual a ser usado para o ambiente de conversão.|
|VirtualMachine::Username |     [opcional] O nome de usuário para a máquina Virtual a ser usado para o ambiente de conversão.|
|PackageInformation::PackageName |      O nome do pacote para o seu pacote MSIX.|
|PackageInformation::PackageDisplayName |       O nome de exibição do pacote para o seu pacote MSIX.|
|PackageInformation::PublisherName |        O publicador para o seu pacote MSIX.|
|PackageInformation::PublisherDisplayName |     O nome de exibição do publicador para o seu pacote MSIX.|
|PackageInformation::Version |      O número de versão do pacote MSIX.|
|PackageInformation:: MainPackageNameForModificationPackage |       [opcional] O nome de identidade do pacote do nome do pacote principal. Isso é usado ao criar um pacote de modificação que leva uma dependência em um aplicativo principal (pai).|
|Aplicativos |     [opcional] 0 ou mais elementos do aplicativo para configurar as entradas de aplicativo em seu pacote MSIX.|
|Application::Id |      A ID do aplicativo para seu aplicativo MSIX. Essa ID será usada para a entrada do aplicativo detectado que corresponde a ExecutableName especificado. Você pode ter vários valores de ID do aplicativo para executáveis no pacote.<br/><br/>Esse valor é o identificador exclusivo do aplicativo dentro do pacote. Às vezes, esse valor é referenciado como o identificador de pacote relativo de aplicativo (PRAID). A ID deve ser exclusiva dentro do pacote (a mesma ID não pode ser usada mais de uma vez no mesmo pacote). No entanto, a ID não deve ser exclusivo globalmente. Pode haver outro pacote no sistema que usa a mesma ID.<br/><br/>Essa cadeia de caracteres contém campos de alfa-numéricos separados por pontos. Cada campo deve começar com um caractere alfabético de ASCII. Você não pode usá-las como valores de campo: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8", and "LPT9".|
|Application::ExecutableName |      O nome do executável do aplicativo de MSIX que será adicionado ao manifesto do pacote. A entrada de aplicativo correspondente será ignorada não se for detectado nenhum aplicativo com esse nome.|
|Application::Description |     [opcional] A descrição do aplicativo para seu aplicativo MSIX. Se não usado, o DisplayName do aplicativo será usado. Essa descrição será usada para a entrada do aplicativo detectado que corresponde a ExecutableName especificado|
|Application::DisplayName    |  O nome de exibição do aplicativo para seu pacote MSIX. Este nome de exibição será usado para a entrada do aplicativo detectado que corresponde a ExecutableName especificado|
|Funcionalidades |     [opcional] 0 ou mais elementos de recurso para adicionar recursos personalizados para seu pacote MSIX. o recurso de "runFullTrust" é adicionado por padrão durante a conversão.|
|Capability::Name | A capacidade de adicionar ao seu pacote MSIX.

