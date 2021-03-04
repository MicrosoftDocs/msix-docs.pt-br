---
title: Usar a Ferramenta de Empacotamento MSIX em um ambiente desconectado
description: Este artigo descreve como adquirir todos os ativos necessários para a ferramenta de empacotamento MSIX se você estiver em um ambiente desconectado.
ms.date: 02/05/2020
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: 9663ab9f43f0e9a36ca23cad2bce584a49dd0415
ms.sourcegitcommit: f0d0dd5275990c50ca4a72a9c6a689ba604aba38
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101841141"
---
# <a name="using-the-msix-packaging-tool-in-a-disconnected-environment"></a>Usar a Ferramenta de Empacotamento MSIX em um ambiente desconectado

Embora tenhamos muito fácil para os usuários adquirirem a ferramenta de empacotamento MSIX por meio da Microsoft Store, mas sabemos que nem todos têm a loja ou um ambiente conectado onde desejam executar conversões. Isso só está disponível para nossas versões públicas, não para as versões do [programa Insider](insider-program.md) .

## <a name="get-the-msix-packaging-tool"></a>Obter a Ferramenta de Empacotamento de MSIX

A Ferramenta de Empacotamento MSIX pode ser baixada para uso offline na empresa no [portal da Web](https://businessstore.microsoft.com/store) do Microsoft Store para Empresas. Saiba mais sobre a distribuição offline [aqui](/microsoft-store/distribute-offline-apps).

Caso tenha problemas com a cópia offline da ferramenta de empacotamento, verifique se você tem a [cópia offline da licença](/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app) para a ferramenta. 

Após obter a versão offline do aplicativo é possível usar o [PowerShell](/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) para adicionar o pacote do aplicativo e a licença ao seu computador.

#### <a name="example-of-offline-installation"></a>Exemplo de instalação offline
```
PS C:\> Add-AppxProvisionedPackage -Path C:\offline -PackagePath C:\MSIX\MyPackage.msix -LicensePath C:\MSIX\MyLicense.xml
```

## <a name="get-the-msix-packaging-tool-driver"></a>Obter o driver da ferramenta de empacotamento MSIX

O driver da ferramenta de empacotamento MSIX é fornecido como um pacote de [fod (recurso sob demanda)](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) do Windows Update e não será instalado se o serviço de Windows Update estiver desabilitado no computador ou as configurações do Windows Insider Flight Ring não corresponderem à compilação do sistema operacional do computador.

Se você estiver em um ambiente corporativo com o Windows Server Update Services (WSUS) ou o Systems Center (agora Microsoft Endpoint Manager), talvez seja necessário modificar a [configuração padrão](/windows/deployment/update/fod-and-lang-packs)ou apenas baixar e instalar o fod manualmente:

- Baixe o arquivo FOD. cab para [Windows 10, versão 1809, x64](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) ou [Windows 10, versão 1809, x86](https://download.microsoft.com/download/9/9/4/9948d09d-af25-45a5-b01f-cc4bcf05f5bf/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)
- Baixe o arquivo FOD. cab para [Windows 10, versão 1903, x64](https://download.microsoft.com/download/5/2/e/52ec35e9-3b50-47b2-879d-c815a93bc3fc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) ou [Windows 10, versão 1903, x86](https://download.microsoft.com/download/2/c/3/2c3a78a2-4d64-426a-976d-dfe4805110cc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab) **Observação: isso também funcionará para o Windows 10, versão 1909**
- Baixe o arquivo FOD. cab para [Windows 10, versão 2004, x64](https://download.microsoft.com/download/a/f/1/af16ad00-3b28-4c8a-9765-1e14a21e93d2/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) ou [Windows 10, versão 2004, x86](https://download.microsoft.com/download/e/5/7/e57f0cec-807b-403e-9ac8-abb2799d09e5/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)
- Os pacotes FOD (Recurso sob Demanda) obtidos individualmente podem ser instalados usando [opções de linha de comando do DISM](/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). Em uma janela do PowerShell com privilégios elevados, digite: ```Dism /Online /add-package /packagepath:(path)```

Os administradores de TI também podem criar um [Repositório de recursos lado a lado (pasta compartilhada)](/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) para permitir o acesso ao FOD do driver da Ferramenta de Empacotamento MSIX. Encontre mais detalhes ao final desta [postagem no blog](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Caso contrário, se você tiver acesso aos canais empresariais ou do OEM, poderá obter o driver da mídia de Recursos sob Demanda do Windows 10 em uma das seguintes fontes:

- [VLSC (centro de serviços de licenciamento por volume)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): o acesso à licença por volume é necessário.
- [Portal OEM](https://www.microsoftoem.com): é necessário acesso ao OEM.
- [Download do MSDN](https://my.visualstudio.com/Downloads/Featured): a assinatura do MSDN é necessária.
