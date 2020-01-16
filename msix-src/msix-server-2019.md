---
title: Suporte do MSIX no servidor 2019
description: Este artigo fornece detalhes sobre como o suporte do MSIX no servidor 2019
ms.date: 04/12/2019
ms.topic: article
author: dianmsft
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 423c58ab10a3e30558b9076c0d2a6c2e1ded9107
ms.sourcegitcommit: 0bd5ebc32feba8a4f4669f232129a8953d5cf773
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030172"
---
# <a name="msix-support-on-server-2019"></a>Suporte do MSIX no servidor 2019

Agora há suporte para MSIX na versão do Windows Server 2019 (LTSC) com a experiência desktop. O Windows Server 2019 é baseado no Windows 10 1809, onde a maioria dos recursos de MSIX estão disponíveis e você poderá aproveitá-los.
 
O suporte permite que você distribua os mesmos pacotes MSIX em sua empresa em SKUs de cliente e de servidor, instalando por meio do **PowerShell** ou instalando diretamente por meio de **APIs do Gerenciador de pacotes**. 
 
## <a name="considerations"></a>Considerações
1. O Microsoft Store e o Microsoft Store for Business não têm suporte no Windows Server, portanto, todos os aplicativos precisarão ser instalados usando o **PowerShell**.
2. Como o Windows Server 2019 é baseado no Windows 10 1809, ele dá suporte a aplicativos que têm uma versão de destino mínima de **10.0.17763** ou inferior.
3. Além disso, as seguintes APIs e estruturas não estão disponíveis no SKU do servidor:
- Estrutura de Microsoft Advertising
- Localização geográfica, pesquisa da Cortana
- Compras no aplicativo

4. As seguintes estruturas não estão instaladas no SKU do servidor, portanto, verifique se elas estão instaladas com o aplicativo quando necessário: 
- Tempo de execução do .NET
- .NET Framework
- VCLibs.

5. Não há suporte para as seguintes extensões MSIX: 
- Windows. firewallRules
- Windows. appExecutionAlias
- Windows. COMServer
- Windows. cominterface
- Windows. posPaymentConnector
- Windows. dataprotection.

> [!NOTE]
> Como o Windows Server 2019 com a experiência desktop não oferece suporte a todos os recursos de SKUs de cliente do Windows 10, recomendamos que você teste aplicativos no servidor antes de implantar em produção.
