---
title: Instalação remota de conversão na ferramenta de empacotamento MSIX
description: Instruções de instalação para conversão remoto
author: c-don
ms.author: cdon
ms.date: 02/26/2019
ms.topic: article
keywords: Ferramenta de empacotamento MSIX, FUTUR, MSIX, IP remoto
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 19a6c4741293835e1494e2796e2ef84f63b63079
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900358"
---
# <a name="setup-instructions-for-remote-machine-conversions"></a>Instruções de instalação para conversões de máquina remota 

> [!NOTE]
> Conversões de máquina remota é um novo recurso que só está disponível no build de versão prévia do insider. 
>
> Você pode unir as [MSIX empacotamento ferramenta Insider Preview Program](insider-program.md) para obter acesso a esse recurso. 

Nesta versão prévia do [ferramenta de empacotamento MSIX](insider-program.md#current-insider-preview-build), habilitamos a capacidade de se conectar a um computador remoto para executar a conversão em. Há algumas etapas que você precisará levar antes de começar com conversões remotas.  

Comunicação remota do PowerShell deve ser habilitada no computador remoto para acesso seguro. Você também deve ter uma conta de administrador para o computador remoto.  Se você quiser se conectar usando um endereço IP, siga as instruções para se conectar a um computador remoto associado de fora do domínio. 

## <a name="connecting-to-a-remote-machine-in-a-trusted-domain"></a>Conectar-se a um computador remoto em um domínio confiável 

Para habilitar a comunicação remota do PowerShell, execute o seguinte no computador remoto de um **admin** janela do PowerShell: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck 
```

Certifique-se de entrar no seu computador ingressado no domínio usando uma conta de domínio e não uma conta local ou você precisará seguir o conjunto de instruções para um computador associado de fora do domínio. 

### <a name="port-configuration"></a>Configuração de porta 

Se o computador remoto é parte de um grupo de segurança (por exemplo, o Azure), você deve configurar suas regras de segurança de rede para acessar o servidor MSIX ferramenta de empacotamento.  

#### <a name="azure"></a>Azure 

1. No Portal do Azure, acesse **Networking** > **adicionar porta de entrada** 
2. Clique em **básica**
3. Campo de serviço deve permanecem definido como **personalizado**
4. Defina o número de porta para **1599** (ferramenta de empacotamento MSIX valor padrão da porta – isso pode ser alterado nas configurações da ferramenta) e dê à regra um nome (por exemplo, AllowMPTServerInBound) 

#### <a name="other-infrastructure"></a>Outra infra-estrutura 

Verifique se a configuração de porta do servidor é alinhada com o valor de porta da ferramenta de empacotamento MSIX (valor padrão da ferramenta de empacotamento MSIX porta é 1599 – isso pode ser alterado nas configurações da ferramenta) 

## <a name="connecting-to-a-non-domain-joined-remote-machineincludes-ip-addresses"></a>Conectar-se ao não-domínio ingressado no computador remoto (inclui os endereços IP) 

Para um computador associado ao fora do domínio, você deve ser configurado com um certificado para se conectar via HTTPS. 

1. Habilitar a comunicação remota do PowerShell e regras de firewall apropriadas, executando o seguinte no computador remoto em um **admin** janela do PowerShell: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck  

New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled  True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP 
```
 
2. Gerar um certificado autoassinado, defina HTTPS do WinRM, configuração e exportar o certificado 

``` PowerShell
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My -KeyExportPolicy NonExportable).Thumbprint 

$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername"";CertificateThumbprint=""$thumbprint""}" 

cmd.exe /C $command 

Export-Certificate -Cert Cert:\LocalMachine\My\$thumbprint -FilePath <path_to_cer_file> 
```

3. Em seu computador local, copie o certificado exportado e instalá-lo no armazenamento raiz confiável 

``` PowerShell
Import-Certificate -FilePath <path> -CertStoreLocation Cert:\LocalMachine\Root 
``` 

### <a name="port-configuration"></a>Configuração de porta 

Se o computador remoto é parte de um grupo de segurança (por exemplo, o Azure), você deve configurar suas regras de segurança de rede para acessar o servidor MSIX ferramenta de empacotamento.  

#### <a name="azure"></a>Azure 

Siga as instruções para [adicionar uma porta personalizada](#azure) para a ferramenta de empacotamento MSIX, bem como adicionar uma regra de segurança de rede para HTTPS do WinRM 

1. No Portal do Azure, acesse **Networking** > **adicionar porta de entrada** 
2. Clique em **básica** 
3. Defina o campo de serviço como **WinRM**

#### <a name="other-infrastructure"></a>Outra infra-estrutura 

Verifique se a configuração de porta do servidor é alinhada com o valor de porta da ferramenta de empacotamento MSIX (valor padrão da ferramenta de empacotamento MSIX porta é 1599 – isso pode ser alterado nas configurações da ferramenta) 
