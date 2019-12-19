---
title: Atualize seu MSIX existente para dar suporte ao MSIX Core
description: Este artigo fornece uma visão geral do MSIX Core, que oferece suporte MSIX para o Windows 7 SP1, Windows 8.1, atualmente com suporte no Windows Server (com experiência desktop) e versões do Windows 10 anteriores à 1709 (atualização de aniversário de outono).
ms.date: 04/12/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: a662ac31dce888c7dc373e546c7a70e1c62e4653
ms.sourcegitcommit: 8aafaa9ac4087ef2e95030343add8fe2ee1cccc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/18/2019
ms.locfileid: "75186782"
---
# <a name="update-your-existing-msix-to-support-msix-core"></a>Atualize seu MSIX existente para dar suporte ao MSIX Core 
Para usar seus pacotes MSIX existentes no Windows 7 SP1, Windows 8.1, atualmente com suporte no Windows Server (com experiência desktop) e versões do Windows 10 anteriores a 1709 (atualização de aniversário de outono), você precisará atualizar o manifesto do pacote MSIX. Os aplicativos empacotados como MSIX devem ser compatíveis com o sistema operacional no qual estão sendo implantados.  O manifesto do pacote MSIX deve conter um **TargetDeviceFamily** apropriado com o nome MSIXCore. desktop e uma MinVersion que corresponda ao número de Build do sistema operacional.  Certifique-se também de incluir a entrada relevante do Windows 10 1709 e posterior, para que o aplicativo seja implantado corretamente em sistemas operacionais que dão suporte nativo a MSIX.

Exemplo para o Windows 7 SP1 como uma versão mínima:

```
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

Todos os aplicativos MSIXCore. desktop serão implantados no Windows Server com sistemas operacionais baseados na experiência desktop com o mesmo número de Build.  Se o aplicativo for destinado apenas a um sistema operacional de servidor, use o TargetDeviceFamily de MSIXCore. Server.  Não há suporte para a implantação do Windows Server Core.

## <a name="supported-versions-of-windows-with-msix-core"></a>Versões com suporte do Windows com o MSIX Core 

| Nome | Versão |
|------|---------|
| Windows 7, service pack 1| 6.1.7601.0|
| Windows 8.1 (atualização 1) |6.3.9600.0|
| Windows 10 2015 LTSB (1507)|10.0.10240.0|
| Windows 10 2016 LTSB (1607)|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
