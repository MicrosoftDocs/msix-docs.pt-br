---
title: Suporte para MSIX no Server 2019
description: Este artigo fornece detalhes sobre o suporte para MSIX no Server 2019
ms.date: 01/16/2020
ms.topic: article
author: dianmsft
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d5da083602173560a9fa481f138e108e851d84fc
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "76248418"
---
# <a name="msix-support-on-windows-server-2019"></a>Suporte ao MSIX no Windows Server 2019

Agora há suporte para MSIX no Windows Server 2019 (LTSC) com a Experiência Desktop. Esse suporte permite que você distribua os mesmos pacotes MSIX na sua empresa em SKUs de cliente e de servidor, fazendo a instalação por meio do **PowerShell** ou diretamente por meio da [API do Gerenciador de Pacotes](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager).

## <a name="considerations"></a>Considerações

* Não há suporte para a Microsoft Store e para a Microsoft Store para Empresas no Windows Server 2019. Todos os aplicativos precisam ser instalados por meio do **PowerShell**.
* Como o Windows Server 2019 é baseado no Windows 10, versão 1809, ele dá suporte aos aplicativos que têm uma versão de destino mínima **10.0.17763** ou inferior.
* As seguintes APIs e estruturas não estão disponíveis no Windows Server 2019:
  * Microsoft Advertising
  * Geolocalização
  * Pesquisa da Cortana
  * Compras no aplicativo
* As estruturas a seguir não estão instaladas no Windows Server 2019. É sua responsabilidade verificar se elas estão instaladas no aplicativo quando necessário.
  * Runtime do .NET
  * .NET Framework
  * VCLibs
* Não há suporte para as seguintes extensões de pacote no Windows Server 2019:
  * windows.firewallRules
  * windows.appExecutionAlias
  * windows.comServer
  * windows.comInterface
  * windows.posPaymentConnector
  * windows.dataProtection

> [!NOTE]
> Como o Windows Server 2019 com a Experiência Desktop não dá suporte a todos os recursos de SKUs de cliente do Windows 10, recomendamos que você teste aplicativos no Windows Server 2019 antes de implantá-los em produção.
