---
title: Scripts de conversão em lote MSIX
description: Este artigo fornece detalhes sobre os scripts de conversão em lote no kit de ferramentas do MSIX.
ms.date: 1/23/2020
ms.topic: article
keywords: Windows 10, MSIX
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0b0329df39d55fff04b6bdf33a5ec3865a823b99
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073562"
---
# <a name="msix-batch-conversion-scripts"></a>Scripts de conversão em lote MSIX

Os [scripts de conversão em lote](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion) no kit de ferramentas MSIX podem ser usados para automatizar a conversão de aplicativos do Windows no formato de pacote MSIX. A lista de aplicativos e seus detalhes são fornecidos no script **entry. ps1** .

## <a name="syntax"></a>Sintaxe

```powershell
entry.ps1
```

## <a name="description"></a>Descrição

Este é um conjunto de scripts do PowerShell que fornece a capacidade de empacotar aplicativos em massa no formato de pacote MSIX. Esses scripts serão conectados a uma máquina virtual local ou a um computador remoto que será usado para empacotar cada aplicativo.

Os aplicativos que estão sendo empacotados no formato de aplicativo MSIX serão convertidos na ordem em que foram inseridos no script **entry. ps1** . Os computadores remotos listados no script **entry. ps1** serão usados para empacotar os aplicativos no formato MSIX serão usados singularmente. As máquinas virtuais podem ser usadas várias vezes para empacotar diferentes aplicativos no formato de aplicativo MSIX.

Antes de executar o script, você deve primeiro adicionar os aplicativos que deseja converter para a variável `conversionsParameters` no script. Vários aplicativos podem ser adicionados à variável. O script aproveita o aplicativo e as máquinas remotas/virtuais para criar um arquivo XML formatado para atender aos requisitos da [ferramenta de empacotamento MSIX](..\packaging-tool\mpt-overview.md) (MsixPackagingTool. exe). Depois de criar o arquivo XML, o script **run_job. ps1** é executado em um novo processo do PowerShell que executa o MsixPackagingTool. exe no dispositivo de destino para converter o aplicativo e colocá-lo na pasta **.\Out** localizada na pasta de execução de script.

## <a name="example"></a>{1&gt;Exemplo&lt;1}

```powershell
PS C:\> entry.ps1
```

Desse exemplo executa o script **entry. ps1** . Esse script converte os aplicativos especificados na variável `conversionsParameters` em pacotes MSIX. Os aplicativos são convertidos usando as máquinas virtuais ou máquinas remotas indicadas nas variáveis *virtualMachines* e *remoteMachines* .

## <a name="parameters"></a>Parâmetros

### <a name="virtualmachines"></a>VirtualMachines

O parâmetro `virtualMachines` é uma matriz que contém o nome e as credenciais das máquinas virtuais a serem conectadas e acessadas ao empacotar um aplicativo no formato MSIX.

* **Tipo:** Variedade
* **Necessário:** Não

```

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

A máquina virtual especificada será usada para empacotar aplicativos no formato MSIX. Esta máquina virtual será conectada usando as credenciais inseridas quando solicitado (prompt aparecerá diretamente após a execução do script **. ps1** Execution).

### <a name="remotemachines"></a>remoteMachines

O parâmetro `remoteMachines` é uma matriz que contém o nome e as credenciais das máquinas remotas a serem conectadas e acessadas ao empacotar um aplicativo no formato MSIX. Os computadores remotos especificados serão dispositivos de uso único usados para empacotar um único aplicativo.

As máquinas remotas devem estar acessíveis e podem ser descobertas na rede.

* **Tipo:** Variedade
* **Necessário:** Não

```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

A máquina remota especificada será usada para empacotar um único aplicativo no formato MSIX. Este computador remoto será conectado ao usando as credenciais inseridas quando solicitado (prompt aparecerá diretamente após a execução do script **. ps1** Execution).

Verifique se o nome de domínio totalmente qualificado ou o alias voltado externamente do dispositivo está resolvido antes da execução do script **entry. ps1** .

### <a name="conversionsparameters"></a>conversõesparameters

O parâmetro `conversionsParameters` é uma matriz que contém informações sobre os aplicativos que você deseja converter em formato MSIX. Cada aplicativo na matriz será analisado individualmente e executado por meio da conversão de pacote MSIX em um computador remoto ou máquina virtual. Os aplicativos serão convertidos na ordem em que aparecem no script. Se a conversão para o formato MSIX falhar, o script não tentará converter novamente o aplicativo em um computador remoto ou máquina virtual diferente.

* **Tipo:** Variedade
* **Necessário:** Ok

```powershell
$conversionsParameters = @(
    @{
        InstallerPath = "Path\To\Your\Installer\YourInstaller.msi"; # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                                    # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                            # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                           # Certificate Publisher information
        PublisherDisplayName = "YourCompany";                       # Application Publisher name
        PackageVersion = "1.0.0.0"                                  # MSIX Application version (must contain 4 octets).
    }
)
```

As informações do aplicativo fornecidas na variável `conversionsParameters` serão usadas para gerar um arquivo XML com todos os detalhes do aplicativo necessários. Depois de criar o arquivo XML, o script passará o arquivo XML para a [ferramenta de empacotamento MSIX](..\packaging-tool\mpt-overview.md) (MsixPackagingTool. exe) para ser empacotado.
