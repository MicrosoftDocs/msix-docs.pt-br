---
title: Plataformas compatíveis
description: Este artigo descreve a plataforma com suporte para MSIX.
author: dianmsft
ms.date: 12/02/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3e01fa41354e8c6fa0b1d1096a597dc0b16fa9a9
ms.sourcegitcommit: 536d6969cde057877ecdd8345cfb0dc12c9582f2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77779074"
---
# <a name="supported-platforms"></a>Plataformas compatíveis

Atualmente, o MSIX tem suporte nas seguintes versões do Windows:

* Windows 10, versão 1709 e posterior.
* Windows Server 2019 LTSC e posterior.
* Windows Enterprise 2019 LTSC e posterior.

Este artigo descreve como os principais recursos do MSIX têm suporte nessas versões do Windows.

> [!NOTE]
> O Windows Server 2019 LTSC e o Windows Enterprise 2019 LTSC exigem que o aplicativo **instalador de aplicativo** dê suporte ao clique duas vezes em instalar ou instalar diretamente de um site para. msix,. msixbundle,. Appx ou. appxbundle. Sem o aplicativo, os pacotes podem ser instalados por meio do PowerShell, API ou usam um produto de gerenciamento de sistemas com suporte. Para obter mais considerações sobre o Windows Server 2019 LTSC, consulte [Este artigo](msix-server-2019.md).

> [!NOTE]
> Para versões do Windows anteriores ao Windows 10 versão 1709, use o [MSIX Core](msix-core/msixcore.md) para instalar pacotes MSIX.

## <a name="msix-feature-support"></a>Suporte ao recurso do MSIX

A tabela a seguir mostra quais recursos e cenários do MSIX têm suporte em diferentes versões do Windows.

> [!div class="mx-tableFixed"]
| Recursos | 1709 | 1803 | 1809 | 1903 | 1909 | 2004| Windows Server 2019 LTSC | Windows Enterprise 2019 LTSC|
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Instalação e desinstalação do MSIX nativo | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark: |
| [Suporte ao arquivo do instalador do aplicativo](app-installer/installing-windows10-apps-web.md)| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Identidade para aplicativos de desktop empacotados | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Permitir elevação | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Pacotes de modificação | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Forçar atualização de qualquer downgrade de versão |  :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Estrutura de suporte do pacote (PSF) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:|  :heavy_check_mark: | :heavy_check_mark:|  
| Serviços Windows | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x:| :x: | 
| Adiar sinalizador de registro |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |
| Forçar provisionamento |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |

## <a name="package-format-support"></a>Suporte a formato de pacote

A tabela a seguir mostra quais formatos de pacote têm suporte em diferentes versões do Windows 10.

| Formato do pacote | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .msixbundle| :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:|
| .appx | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .appxbundle | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## <a name="microsoft-store"></a>Microsoft Store

A tabela a seguir mostra quais recursos de Microsoft Store têm suporte em diferentes versões do Windows 10.

| Recursos | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Publicação             | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Notificação de atualização| :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Instalação de streaming | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| 
| Atualizações Delta | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

> [!NOTE]
> . Appx ou. appxbundle funcionará para todas as versões do Windows 10 listadas acima. A tabela reflete apenas o comportamento. msix ou. msixbundle.

### <a name="microsoft-store-submissions"></a>Envios de Microsoft Store

A versão mínima compatível do sistema operacional de um pacote MSIX está listada no arquivo de manifesto do pacote como `MinVersion` no elemento `TargetDeviceFamily`. Por exemplo, um pacote MSIX pode listar `MinVersion="10.0.17701.0"` como a versão mínima com suporte, o que significa que o pacote MSIX pode ser executado neste e em versões posteriores do sistema operacional.

No Windows 10, versões 1709, 1803 e 1809, damos suporte aos cenários de implantação corporativos convencionais. Isso inclui a instalação por meio do Microsoft Endpoint Configuration Manager, Microsoft Intune, PowerShell ou clique duas vezes em instalação.

Atualmente, a instalação do MSIX por meio do Microsoft Store e Microsoft Store for Business requer o Windows 10, versão 1809 e posterior.
