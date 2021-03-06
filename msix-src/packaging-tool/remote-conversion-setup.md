---
title: Instalação de conversão remota na Ferramenta de Empacotamento MSIX
description: Este artigo descreve como configurar e conectar-se a um computador remoto para executar conversões de aplicativo usando a ferramenta de empacotamento MSIX.
ms.date: 02/26/2019
ms.topic: article
keywords: MSIX, MPT, Ferramenta de Empacotamento MSIX, IP remoto
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: a13b2a2725cc8c3750cbd22d4238ae41f5406fdc
ms.sourcegitcommit: 4d912f89e385268757e87bf8fd9ca1828b99e109
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77544768"
---
# <a name="setup-instructions-for-remote-machine-conversions"></a>Instruções de instalação para conversões de computador remoto

Conectar-se a um computador remoto é uma opção para garantir que você esteja seguindo a recomendação de [práticas recomendadas](prepare-your-environment.md) para seu ambiente de conversão, pois pode ser um ambiente mais limpo do que o computador local. Há algumas etapas que você precisará seguir antes de começar a usar as conversões remotas.  

A comunicação remota do PowerShell precisa ser habilitada no computador remoto para acesso seguro. Você também deve ter uma conta de administrador para seu computador remoto.  Caso deseje se conectar usando um endereço IP, siga as instruções para se conectar a um computador remoto não ingressado no domínio.

## <a name="connecting-to-a-remote-machine-in-a-trusted-domain"></a>Como se conectar a um computador remoto em um domínio confiável

Para habilitar a comunicação remota do PowerShell, execute o seguinte no computador remoto de uma janela do PowerShell como **administrador**: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck
```

Entre no computador ingressado no domínio usando uma conta de domínio e não uma conta local ou você precisará seguir as instruções de configuração para um computador não ingressado no domínio.

### <a name="port-configuration"></a>Configuração de porta

Se o computador remoto fizer parte de um grupo de segurança (como o Azure), você precisará configurar as regras de segurança de rede para acessar o servidor da Ferramenta de Empacotamento MSIX.  

#### <a name="azure"></a>Azure

1. No portal do Azure, acesse **Rede** > **Adicionar porta de entrada**
2. Clique em **Básico**
3. O campo de serviço deve permanecer definido como **Personalizado**
4. Defina o número da porta como **1599** (valor da porta padrão da Ferramenta de Empacotamento MSIX – isso pode ser alterado nas Configurações da ferramenta) e dê um nome à regra (por exemplo, AllowMPTServerInBound)

#### <a name="other-infrastructure"></a>Outras infraestruturas

Verifique se a configuração de porta do servidor está alinhada com o valor de porta da Ferramenta de Empacotamento MSIX (o valor padrão da porta da Ferramenta de Empacotamento MSIX é 1599 – isso pode ser alterado nas Configurações da ferramenta)

## <a name="connecting-to-a-non-domain-joined-remote-machineincludes-ip-addresses"></a>Como se conectar a um computador remoto não ingressado no domínio (inclui os endereços IP)

Para um computador não ingressado no domínio, é necessário configurar um certificado para se conectar via HTTPS.

1. Habilite a comunicação remota do PowerShell e as regras de firewall apropriadas executando o seguinte no computador remoto em uma janela do PowerShell como **administrador**:

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck  

New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled  True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
```
 
2. Gerar um certificado autoassinado, definir a configuração HTTPS do WinRM e exportar o certificado

``` PowerShell
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My -KeyExportPolicy NonExportable).Thumbprint

$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername"";CertificateThumbprint=""$thumbprint""}"

cmd.exe /C $command

Export-Certificate -Cert Cert:\LocalMachine\My\$thumbprint -FilePath <path_to_cer_file>
```

3. No computador local, copie o certificado exportado e instale-o no repositório de raiz confiável

``` PowerShell
Import-Certificate -FilePath <path> -CertStoreLocation Cert:\LocalMachine\Root
```

### <a name="port-configuration"></a>Configuração de porta 

Se o computador remoto fizer parte de um grupo de segurança (como o Azure), você precisará configurar as regras de segurança de rede para acessar o servidor da Ferramenta de Empacotamento MSIX.  

#### <a name="azure"></a>Azure

Siga as instruções para [adicionar uma porta personalizada](#azure) à Ferramenta de Empacotamento MSIX, bem como adicionar uma regra de segurança de rede ao HTTPS do WinRM

1. No portal do Azure, acesse **Rede** > **Adicionar porta de entrada**
2. Clique em **Básico**
3. Defina o campo Serviço como **WinRM**

#### <a name="other-infrastructure"></a>Outras infraestruturas 

Verifique se a configuração de porta do servidor está alinhada com o valor de porta da Ferramenta de Empacotamento MSIX (o valor padrão da porta da Ferramenta de Empacotamento MSIX é 1599 – isso pode ser alterado nas Configurações da ferramenta)
