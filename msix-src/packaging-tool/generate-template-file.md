---
title: Como gerar um arquivo de modelo para conversões de linha de comando
description: Este artigo descreve como gerar um arquivo de modelo para conversões de linha de comando.
ms.date: 01/28/2020
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: 9e5fd913de5e1c057e335c8b3102a14a9b75adf3
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073502"
---
# <a name="how-to-generate-a-template-file-for-command-line-conversions"></a>Como gerar um arquivo de modelo para conversões de linha de comando

Com a ferramenta de empacotamento MSIX, você pode executar a conversão de duas maneiras: por meio da interface do usuário interativa ou da nossa opção de linha de comando. Ao usar a linha de comando, você precisa fornecer um arquivo de modelo para que a conversão funcione com suas configurações e necessidades específicas. Este artigo ajudará você a guiá-lo pelo processo de geração de um arquivo de modelo que funciona para você.

Há duas maneiras de obter um arquivo de modelo que funciona para você:

* Você pode usar a interface do usuário da ferramenta de empacotamento MSIX. Nas configurações da ferramenta, você pode especificar que deseja gerar um arquivo de modelo de conversão com cada pacote MSIX que você criar.
* Você pode usar um [modelo de exemplo](#sample-conversion-template-file) e inserir manualmente as configurações necessárias para cada conversão.

## <a name="generate-a-conversion-template-file-from-the-msix-packaging-tool"></a>Gerar um arquivo de modelo de conversão da ferramenta de empacotamento MSIX

- Iniciar a ferramenta de empacotamento MSIX
- Vá para as configurações no canto superior direito do aplicativo
- Verifique se o ' gerar arquivo de modelo de conversão para cada pacote '
- Faça outras alterações ou modificações nas configurações necessárias (por exemplo, itens de exclusão, códigos de saída)
- Salvar as configurações

- Percorrer o fluxo de trabalho do pacote de aplicativos usando um instalador
    - Se você não selecionar um instalador, não será possível gerar um arquivo de modelo de conversão
    - Se você estiver usando um exe, será necessário passar um sinalizador silencioso para o instalador a fim de gerar o arquivo de modelo de conversão
- No final da conversão, você terá um arquivo de modelo configurado com base no instalador escolhido e suas configurações atuais que agora podem ser reutilizadas para conversões futuras
    - Por padrão, o arquivo de modelo de conversão será salvo no mesmo local que o pacote MSIX, mas você pode especificar um local de salvamento separado para o arquivo de modelo na página Criar pacote
    - Você ainda precisará fazer algumas modificações com base no que MSIX você deseja que a saída no final de cada conversão


## <a name="manually-edit-the-conversion-template-file"></a>Editar manualmente o arquivo de modelo de conversão

Você pode editar manualmente os parâmetros do modelo para o arquivo de modelo de conversão para gerar um arquivo de modelo que funcione para você. Ao gerar o arquivo de modelo de conversão, preste atenção aos recursos que você adiciona ao arquivo de modelo, pois alguns podem exigir referências de esquema adicionais para funcionar.

### <a name="conversion-template-parameter-reference"></a>Referência de parâmetro de modelo de conversão

Esta é a lista completa de parâmetros que você pode usar no arquivo de modelo de conversão.

|**ConversionSettings** |   **Descrição** |
|---------|---------|
|Configurações:: AllowTelemetry | [opcional] Habilita o log de telemetria para essa invocação da ferramenta.|
|Configurações:: ApplyAllPrepareComputerFixes | [opcional] Aplica todas as correções de preparação do computador recomendadas. Não pode ser definida quando outros atributos são usados.|
|Configurações:: GenerateCommandLineFile | [opcional] Copia a entrada do arquivo de modelo para o diretório SaveLocation para uso futuro.|
|Configurações:: AllowPromptForPassword | [opcional] Instrui a ferramenta a solicitar que o usuário insira senhas para a Máquina Virtual e o certificado de autenticação, caso ele seja necessário e não especificado.|
|Configurações:: EnforceMicrosoftStoreVersioningRequirements | [opcional] Instrui a ferramenta a impor o esquema de controle de versão do pacote necessário para a implantação por meio da Microsoft Store e da Microsoft Store para Empresas.|
|Configurações:: ServerPortNumber| adicional Usado ao se conectar a um computador remoto. Requer v2 do esquema do modelo.|
|Configurações:: AddPackageIntegrity| adicional Adiciona a integridade do pacote a cada MSIX gerada. Requer v5 do esquema do modelo. |
|ValidInstallerExitCodes | [opcional] 0 ou mais elementos ValidInstallerExitCode. Requer v2 do esquema do modelo. |
|ValidInstallerExitCodes:: ValidInstallerExitCode | adicional Especifique os códigos de saída do instalador para os quais a ferramenta talvez não esteja familiarizada ou exija uma reinicialização. Requer v2 do esquema do modelo. |
|ValidInstallerExitCodes:: ValidInstallerExitCode:: reboot | adicional Especifique se um código de saída deve disparar uma reinicialização durante a conversão. Requer V3 do esquema do modelo. |
|ExclusionItems | [opcional] 0 ou mais elementos FileExclusion ou RegistryExclusion. Todos os elementos FileExclusion precisam aparecer antes dos elementos RegistryExclusion.|
|ExclusionItems::FileExclusion | [opcional] Um arquivo a ser excluído para o empacotamento.|
|ExclusionItems::FileExclusion::ExcludePath | Caminho do arquivo a ser excluído para o empacotamento.|
|ExclusionItems::RegistryExclusion | [opcional] Uma chave do Registro a ser excluído para o empacotamento.|
|ExclusionItems::RegistryExclusion:: ExcludePath | Caminho do Registro a ser excluído para o empacotamento.|
|PrepareComputer::DisableDefragService | [opcional] Desabilita o Desfragmentador do Windows enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsSearchService | [opcional] Desabilita o Windows Search enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableSmsHostService | [opcional] Desabilita o Host do SMS enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsUpdateService | [opcional] Desabilita o Windows Update enquanto o aplicativo está sendo convertido. Se for definido como false, substituirá ApplyAllPrepareComputerFixes.|
|SaveLocation | [opcional] Um elemento para especificar a localização de salvamento da ferramenta. Se não for especificado, o pacote será salvo na pasta Área de Trabalho. |
|SaveLocation::PackagePath | [opcional] O caminho para o arquivo ou a pasta em que o pacote MSIX resultante é salvo. |
|SaveLocation::TemplatePath | adicional O caminho para o arquivo ou pasta em que o modelo de linha de comando resultante é salvo. |
|Installer::Path | O caminho para o Instalador de Aplicativo.|
|Installer::Arguments | [opcional] Os argumentos a serem passados para o instalador.  A ferramenta executará de forma automática os instaladores MSI silenciosamente usando o argumento "/qn /norestart INSTALLSTARTMENUSHORTCUTS=1 DISABLEADVTSHORTCUTS=1". Observação: você deve passar os argumentos para forçar o instalador a executar silenciosamente se você estiver usando instaladores. exe.|
|Installer::InstallLocation | adicional O caminho completo para a pasta raiz do aplicativo para os arquivos instalados, se ele foi instalado (por exemplo, "C:\Arquivos de programas (x86) \MyAppInstalllocation").|
|Máquina virtual | [opcional] Um elemento para especificar que a conversão será executada em uma Máquina Virtual local.|
|VirtualMachine:: nome | O nome da máquina virtual a ser usada para o ambiente de conversão.|
|VirtualMachine::Username | O nome de usuário da máquina virtual a ser usada para o ambiente de conversão.|
|RemoteMachine | adicional Um elemento para especificar que a conversão será executada em um computador remoto. Requer v2 do esquema do modelo. |
|RemoteMachine:: ComputerName | O nome do computador remoto a ser usado para o ambiente de conversão. Requer v2 do esquema do modelo. |
|RemoteMachine:: username | O nome de usuário para o computador remoto a ser usado para o ambiente de conversão. Requer v2 do esquema do modelo. |
|RemoteMachine:: EnableAutoLogon | adicional Isso fará o logon automático de volta ao executar uma conversão que exige uma reinicialização em um computador remoto para que sua conversão continue diretamente. Requer V3 do esquema do modelo. |
|PackageInformation::PackageName | O nome do pacote MSIX.|
|PackageInformation::PackageDisplayName | O nome de exibição do pacote MSIX.|
|PackageInformation::PublisherName | O fornecedor do pacote MSIX.|
|PackageInformation::PublisherDisplayName | O nome de exibição do fornecedor do pacote MSIX.|
|PackageInformation::Version | O número de versão do pacote MSIX.|
|PackageInformation::P ackageDescription | adicional A descrição do pacote MSIX. Requer v4 do esquema do modelo. |
|PackageInformation:: MainPackageNameForModificationPackage | [opcional] O nome da identidade do pacote do nome do pacote principal. Isso é usado durante a criação de um pacote de modificação que tem uma dependência de um aplicativo principal (pai).|
|SigningInformation| adicional Um elemento para especificar informações de assinatura para assinatura do Device Guard. Requer v4 do esquema do modelo. |
|SigningInformation:: DeviceGuardSigning | adicional Um elemento para especificar informações de assinatura do Device Guard. Requer v4 do esquema do modelo. |
|DeviceGuardSigning:: Tokenfile | O [token de acesso do AD do Azure necessário](../package/signing-package-device-guard-signing.md#get-an-azure-ad-access-token) para a assinatura do Device Guard no formato JSON. Requer esquema de modelo v4. |
|DeviceGuardSigning:: TimestampUrl | adicional Fornece um carimbo de data/hora no momento da assinatura com o Device Guard para garantir que seu aplicativo será instalado além do tempo de vida do certificado. Requer v4 do esquema do modelo. |
|Aplicativos | [opcional] 0 ou mais elementos Application para configurar as entradas de Application no pacote MSIX.|
|Application::Id | A ID do Aplicativo MSIX. Essa ID será usada para a entrada Application detectada que corresponde ao ExecutableName especificado. Você pode ter vários valores da ID do Aplicativo para executáveis no pacote.<br/><br/>Esse valor é o identificador exclusivo do aplicativo no pacote. Às vezes, esse valor é denominado PRAID (identificador do aplicativo relativo do pacote). A ID precisa ser exclusiva no pacote (a mesma ID não pode ser usada mais de uma vez no mesmo pacote). No entanto, a ID não precisa ser globalmente exclusiva. Pode haver outro pacote no sistema que usa a mesma ID.<br/><br/>Essa cadeia de caracteres contém campos alfanuméricos separados por pontos. Cada campo precisa começar com um caractere alfabético ASCII. Você não pode usá-los como valores de campo: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8" e "LPT9".|
|Application::DisplayName | O nome de exibição do aplicativo para o pacote MSIX. Esse nome de exibição será usado para a entrada do aplicativo detectada que corresponde ao ExecutableName especificado|
|Application::ExecutableName | O nome do executável do aplicativo MSIX que será adicionado ao manifesto do pacote. A entrada de aplicativo correspondente será ignorada se nenhum aplicativo com esse nome for detectado.|
|Application::Description | [opcional] A descrição do aplicativo MSIX. Se não for usado, o Application DisplayName será usado. Essa descrição será usada para a entrada do aplicativo detectada que corresponde ao ExecutableName especificado|
|{1&gt;Capabilities&lt;1} | [opcional] 0 ou mais elementos Capability para adicionar funcionalidades personalizadas ao pacote MSIX. A funcionalidade “runFullTrust” é adicionada por padrão durante a conversão.|
|Capability::Name | A capacidade a ser adicionada ao pacote MSIX. |

### <a name="sample-conversion-template-file"></a>Exemplo de arquivo de modelo de conversão

```xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018"
    xmlns:V2="http://schemas.microsoft.com/msix/msixpackagingtool/template/1904"
    xmlns:V3="http://schemas.microsoft.com/msix/msixpackagingtool/template/1907"
    xmlns:V4="http://schemas.microsoft.com/msix/msixpackagingtool/template/1910"
    xmlns:V5="http://schemas.microsoft.com/msix/msixpackagingtool/template/2001">
<!--Note: You only need to include xmlns:v2 - xmlns:v5 if you are using one of the features that use those schemas -->

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
        EnforceMicrosoftStoreVersioningRequirements="false"
        v2:ServerPortNumber="1599"
        v5:AddPackageIntegrity="true">    

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
    
    <!--Note: Specifying an installer exit code will allow you to automatically trigger a reboot during your conversion
      <v2:ValidInstallerExitCodes>
        <V2:ValidInstallerExitCode ExitCode="3010" V3:Reboot="true"/>
        <V2:ValidInstallerExitCode ExitCode="1641"/>
      </v2:ValidInstallerExitCodes>
    -->
        
    </Settings>

    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute and is optional
    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService="true"/>
    -->

    <SaveLocation
        PackagePath="C:\users\user\Desktop\MyPackage.msix" 
        TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />
    
    <!--NOTE: This section specifies that the conversion will be run on a local Virtual Machine. This is optional if you want to change your conversion environment from the default local machine.
    <VirtualMachine Name="vmname" Username="vmusername"/>
    -->

    <!--NOTE: This section specifies that the conversion will be run on a remote machine.This is optional if you want to change your conversion environment from the default local machine.
    <v2:RemoteMachine ComputerName="vmname" Username="vmusername" v3:EnableAutoLogon="true"/>
    -->

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">

    <!--Note: This is optional, if you want to sign your package with Device Guard signing
        <v4:SigningInformation>
            <v4:DeviceGuardSigning
                Tokenfile="tokenfile.json"
                TimestampUrl="https://mytimestamp.com"/>
        </v4:SigningInformation>
    -->
        
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