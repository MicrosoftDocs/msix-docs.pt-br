---
title: Suporte do MSIX no servidor 2019
description: Este artigo fornece detalhes sobre como o suporte do MSIX no servidor 2019
ms.date: 01/16/2020
ms.topic: article
author: dianmsft
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d5da083602173560a9fa481f138e108e851d84fc
ms.sourcegitcommit: e1da85c80d639646f9c9dd7ab26e7e4bffd1c9dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76248418"
---
# <a name="msix-support-on-windows-server-2019"></a>Suporte do MSIX no Windows Server 2019

O MSIX agora tem suporte no Windows Server 2019 (LTSC) com a experiência desktop. Esse suporte permite que você distribua os mesmos pacotes MSIX em sua empresa em SKUs de cliente e de servidor, instalando por meio do **PowerShell** ou instalando diretamente por meio da [API do Gerenciador de pacotes](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager).

## <a name="considerations"></a>Considerações

* O Microsoft Store e o Microsoft Store para empresas não têm suporte no Windows Server 2019. Todos os aplicativos devem ser instalados usando o **PowerShell**.
* Como o Windows Server 2019 é baseado no Windows 10, versão 1809, ele dá suporte a aplicativos que têm uma versão de destino mínima de **10.0.17763** ou inferior.
* As seguintes APIs e estruturas não estão disponíveis no Windows Server 2019:
  * Microsoft Advertising
  * Geolocalização
  * Pesquisa da Cortana
  * Compra no aplicativo
* As estruturas a seguir não estão instaladas no Windows Server 2019. É sua responsabilidade verificar se eles estão instalados com seu aplicativo quando necessário.
  * Tempo de execução do .NET
  * .NET Framework
  * VCLibs
* As seguintes extensões de pacote não têm suporte no Windows Server 2019:
  * Windows. firewallRules
  * Windows. appExecutionAlias
  * Windows. COMServer
  * Windows. cominterface
  * Windows. posPaymentConnector
  * Windows. dataprotection

> [!NOTE]
> Como o Windows Server 2019 com a experiência desktop não oferece suporte a todos os recursos de SKUs de cliente do Windows 10, recomendamos que você teste aplicativos no Windows Server 2019 antes de implantar em produção.
