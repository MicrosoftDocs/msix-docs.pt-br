---
title: Atualizar seu MSIX existente para oferecer suporte ao MSIX Core
description: Este artigo fornece uma visão geral do MSIX Core, que oferece suporte MSIX para o Windows 7 SP1, Windows 8.1, atualmente com suporte no Windows Server (com experiência desktop) e versões do Windows 10 anteriores à 1709 (atualização de aniversário de outono).
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d21d350e6df7f0b0860a5c0b428a42e8d4517cef
ms.sourcegitcommit: 44b9510ea76623d668d87ddca575a7921c60a19a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75322651"
---
# <a name="update-your-existing-msix-package-to-support-msix-core"></a>Atualize seu pacote MSIX existente para dar suporte ao MSIX Core

Antes de implantar o pacote MSIX com o [MSIX Core](msixcore.md), você deve primeiro atualizar o manifesto do pacote MSIX.

Os aplicativos empacotados como MSIX devem ser compatíveis com o sistema operacional no qual estão sendo implantados. O manifesto do pacote MSIX deve conter um **TargetDeviceFamily** apropriado com o nome **MSIXCore. desktop** e uma **MinVersion** que corresponda ao número de Build do sistema operacional. Certifique-se também de incluir a entrada relevante do Windows 10, versão 1709 e posterior, para que o aplicativo seja implantado corretamente em sistemas operacionais que oferecem suporte nativo a MSIX.

O exemplo a seguir especifica o Windows 7 SP1 como uma versão mínima:

```xml
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

Todos os aplicativos **MSIXCore. desktop** serão implantados no Windows Server com sistemas operacionais baseados na experiência desktop com o mesmo número de Build. Se o aplicativo for destinado apenas a um sistema operacional de servidor, especifique um **TargetDeviceFamily** com o nome **MSIXCore. Server**. Não há suporte para a implantação do Windows Server Core.

## <a name="update-manifest-using-the-msix-packaging-tool"></a>Atualizar o manifesto usando a ferramenta de empacotamento MSIX 
Se você tiver um pacote MSIX, poderá usar a ferramenta de pacote MSIX para atualizar seu pacote para dar suporte ao MSIX Core. Estas são as etapas: 
1. Abrir o aplicativo de **ferramenta de empacotamento MSIX**
2. Selecionar o **Editor de pacote** 
3. Clique em **procurar...** para localizar o pacote
4. Clique em **Abrir pacote**
5. Em **arquivo de manifesto**, clique em **Abrir arquivo**
6. Você está exibindo o manifesto do pacote. Em **dependência** , adicione MSIX Core como uma família de dispositivos de destino (veja acima)
7. Salvar e fechar o manifesto 
8. Desistir do pacote 
9. Clique em **salvar** e selecione se deseja que seu pacote seja incrementado 

## <a name="windows-versions-supported-by-msix-core"></a>Versões do Windows com suporte do MSIX Core

| Nome | Versão |
|------|---------|
| Windows 7, SP 1| 6.1.7601.0|
| Windows 8.1 (atualização 1) |6.3.9600.0|
| Windows 10 2015 LTSB (1507)|10.0.10240.0|
| Windows 10 2016 LTSB (1607)|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| R2 do Windows Server 2012| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
