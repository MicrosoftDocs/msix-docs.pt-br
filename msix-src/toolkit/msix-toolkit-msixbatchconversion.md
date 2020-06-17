---
title: Scripts de conversão em massa do MSIX
description: Este artigo fornece detalhes sobre os scripts de conversão em massa no kit de ferramentas do MSIX.
ms.date: 1/23/2020
ms.topic: article
keywords: Windows 10, msix, msixtoolkit, msix Toolkit, kit de ferramentas, lote, conversão, massa, conversão em massa
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b2af803cc540d393703e68569d8da1a78d478f2d
ms.sourcegitcommit: 6243b7aca6f52f007f4571c835f580f433c31769
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84812785"
---
# <a name="msix-bulk-conversion-scripts"></a>Scripts de conversão em massa do MSIX

Os [scripts de conversão em massa](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion) no kit de ferramentas MSIX podem ser usados para automatizar a conversão de aplicativos do Windows no formato de pacote MSIX. A lista de aplicativos e seus detalhes são fornecidos no script de **entry.ps1** .

## <a name="prepare-machines-for-conversion"></a>Preparar máquinas para conversão
Antes de executar o script de conversão em massa do MSIX Toolkit, para automatizar a conversão de seu aplicativo para o formato de empacotamento do MSIX, os dispositivos que você usará (virtual ou remota) devem ser configurados para permitir a comunicação remota e ter a ferramenta de empacotamento MSIX instalada.

| Termo            | Descrição                                                        |
|-----------------|--------------------------------------------------------------------|
| Máquina host    | Este é o dispositivo que executa os scripts de conversão em massa.          |
| Máquina Virtual | Este é um dispositivo existente no Hyper-V, hospedado no computador host.  |
| Computador remoto  | Essa é uma máquina física ou virtual acessível pela rede. |

### <a name="host-machine"></a>Máquina host
O computador host deve atender aos seguintes requisitos:
* A [ferramenta de empacotamento MSIX](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) deve estar instalada.
* Se as máquinas virtuais estiverem sendo usadas, o Hyper-V deverá ser instalado.
* Se as máquinas remotas estiverem sendo usadas:
    * O dispositivo existe no mesmo domínio que os computadores remotos:
        * Habilitar a Comunicação Remota do PowerShell 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            ```
    * O dispositivo existe em um grupo de trabalho ou em um domínio alternativo como computador (es) remoto (s):
        * Habilitar a comunicação remota do PowerShell 
        * O host confiável do WinRM deve conter o nome do dispositivo ou o endereço IP do computador remoto 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            Set-Item WSMan:\localhost\Client\TrustedHosts -Value <RemoteMachineName>,[<RemoteMachineName>,...]
            ```

### <a name="remote-machine"></a>Computador remoto
O computador remoto deve atender aos seguintes requisitos:
* A [ferramenta de empacotamento MSIX](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) deve estar instalada.
* Se o dispositivo existir no mesmo domínio que o computador host:
    * Habilitar a Comunicação Remota do PowerShell 
    * O WinRM deve estar habilitado 
    * Permitir ICMPv4 por meio do firewall do cliente
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        ```

* Se o dispositivo existir em um grupo de trabalho ou em um domínio alternativo como o computador host:
    * Habilitar a Comunicação Remota do PowerShell 
    * O host confiável do WinRM deve conter o nome do dispositivo ou o endereço IP do computador host
    * Permitir ICMPv4 por meio do firewall do cliente
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        Set-Item WSMan:\localhost\Client\TrustedHosts -Value <HostMachineName>
        
        ```

### <a name="virtual-machine"></a>Máquina Virtual
É recomendável que a imagem de criação rápida do Hyper-V "ambiente de ferramentas de empacotamento MSIX" seja usada, já que ela é pré-configurada para atender a todos os requisitos. A máquina virtual deve estar hospedada no computador host e em execução no Microsoft Hyper-V.

A máquina virtual deve atender aos seguintes requisitos:
* A [ferramenta de empacotamento MSIX](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) deve estar instalada.

## <a name="syntax"></a>Sintaxe

```powershell
entry.ps1
```

## <a name="description"></a>Descrição

Este é um conjunto de scripts do PowerShell que fornece a capacidade de empacotar aplicativos em massa no formato de pacote MSIX. Esses scripts serão conectados a uma máquina virtual local ou a um computador remoto que será usado para empacotar cada aplicativo.

Os aplicativos que estão sendo empacotados no formato de aplicativo MSIX serão convertidos na ordem em que foram inseridos no script de **entry.ps1** . Os computadores remotos listados no script de **entry.ps1** serão usados para empacotar os aplicativos no formato MSIX serão usados singularmente. As máquinas virtuais podem ser usadas várias vezes para empacotar diferentes aplicativos no formato de aplicativo MSIX.

Antes de executar o script, você deve primeiro adicionar os aplicativos que deseja converter para a `conversionsParameters` variável no script. Vários aplicativos podem ser adicionados à variável. O script aproveita o aplicativo e as máquinas remotas/virtuais para criar um arquivo XML formatado para atender aos requisitos da [ferramenta de empacotamento MSIX](..\packaging-tool\mpt-overview.md) (MsixPackagingTool.exe). Depois de criar o arquivo XML, o script de **run_job.ps1** é executado em um novo processo do PowerShell que executa MsixPackagingTool.exe no dispositivo de destino para converter o aplicativo e colocá-lo na pasta **.\Out** localizada na pasta de execução do script.

## <a name="example"></a>Exemplo

```powershell
PS C:\> entry.ps1
```

Desse exemplo executa o script de **entry.ps1** . Esse script converte os aplicativos especificados na `conversionsParameters` variável em pacotes MSIX. Os aplicativos são convertidos usando as máquinas virtuais ou máquinas remotas indicadas nas variáveis *virtualMachines* e *remoteMachines* .

## <a name="parameters"></a>Parâmetros

### <a name="virtualmachines"></a>virtualMachines

O `virtualMachines` parâmetro é uma matriz que contém o nome e as credenciais das máquinas virtuais a serem conectadas e acessadas ao empacotar um aplicativo no formato MSIX.

* **Tipo:** Variedade
* **Obrigatória:** Não

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

A máquina virtual especificada será usada para empacotar aplicativos no formato MSIX. Esta máquina virtual será conectada usando as credenciais inseridas quando solicitado (o prompt aparecerá diretamente após a execução do script **entry.ps1** ). Antes de empacotar um aplicativo no formato de empacotamento MSIX, o script criará um instantâneo da VM do Hyper-V e, em seguida, será restaurado para esse instantâneo depois que o aplicativo tiver sido empacotado.

### <a name="remotemachines"></a>remoteMachines

O `remoteMachines` parâmetro é uma matriz que contém o nome e as credenciais das máquinas remotas a serem conectadas e acessadas ao empacotar um aplicativo no formato MSIX. Os computadores remotos especificados serão dispositivos de uso único usados para empacotar um único aplicativo.

As máquinas remotas devem estar acessíveis e podem ser descobertas na rede.

* **Tipo:** Variedade
* **Obrigatória:** Não

```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

A máquina remota especificada será usada para empacotar um único aplicativo no formato MSIX. Este computador remoto será conectado ao usando as credenciais inseridas quando solicitado (o prompt aparecerá diretamente após a execução do script **entry.ps1** ).

Verifique se o nome de domínio totalmente qualificado ou o alias voltado externamente do dispositivo está resolvido antes da execução do script de **entry.ps1** .

### <a name="signingcertificate"></a>signingCertificate
O `signingCertificate` parâmetro é uma matriz que contém informações relacionadas ao certificado de assinatura de código que será usado para assinar o aplicativo empacotado MSIX. Esse certificado deve ter um nível de criptografia de, no mínimo, SHA256.

* **Tipo:** Variedade
* **Obrigatória:** Não

```powershell
$SigningCertificate = @{
    Password = "Password"; 
    Path = "C:\Temp\ContosoLab.pfx"
}
```
### <a name="conversionsparameters"></a>conversõesparameters

O `conversionsParameters` parâmetro é uma matriz que contém informações sobre os aplicativos que você deseja converter em formato MSIX. Cada aplicativo na matriz será analisado individualmente e executado por meio da conversão de pacote MSIX em um computador remoto ou máquina virtual. Os aplicativos serão convertidos na ordem em que aparecem no script. Se a conversão para o formato MSIX falhar, o script não tentará converter novamente o aplicativo em um computador remoto ou máquina virtual diferente.

* **Tipo:** Variedade
* **Obrigatória:** Sim

```powershell
$conversionsParameters = @(
    ## Use for MSI applications:
    @{
        InstallerPath = "C:\Path\To\YourInstaller.msi";    # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0"                         # MSIX Application version (must contain 4 octets).
    },
    ## Use for EXE or other applications:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement"   # Arguements required by the installer to provide a silent installation of the application.
    },
    ## Creating the Packaged app and Template file in a specific folder path:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement";  # Arguements required by the installer to provide a silent installation of the application.
        SavePackagePath = "Custom\folder\Path";            # Specifies a custom folder path where the MSIX app will be created.
        SaveTemplatePath = "Custom\folder\Path"            # Specifies a custom folder path where the MSIX Template XML will be created.
    }
)
```

As informações do aplicativo fornecidas na `conversionsParameters` variável serão usadas para gerar um arquivo XML com todos os detalhes necessários do aplicativo. Depois de criar o arquivo XML, o script passará o arquivo XML para a [ferramenta de empacotamento MSIX](..\packaging-tool\mpt-overview.md) (MsixPackagingTool.exe) para ser empacotado.

## <a name="logging"></a>Registro em log

O script irá gerar um arquivo de log que descreve o que ocorreu durante a execução do script. O arquivo de log fornecerá detalhes relacionados ao pacote de aplicativos para o formato de empacotamento MSIX e informações relacionadas à progressão de script. Os logs podem ser lidos de qualquer utilitário de texto, mas foram configurados para leitura usando o leitor de log do Trace32. Os erros na execução do script serão realçados como vermelho e os avisos são amarelos. Para obter mais informações sobre o leitor de log 32 de rastreamento, visite [CMTrace](https://docs.microsoft.com/mem/configmgr/core/support/cmtrace) em Microsoft docs.

O arquivo de log é criado no diretório do script `.\logs\BulkConversion.log` .
