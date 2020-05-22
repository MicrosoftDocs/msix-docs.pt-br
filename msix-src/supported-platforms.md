---
title: Plataformas compatíveis
description: Este artigo descreve a plataforma com suporte para MSIX.
author: dianmsft
ms.date: 03/06/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9cece807e2206f754e96f6a394c5da626129c177
ms.sourcegitcommit: d65b3457343e0590f53e36fc2710863cc2f13897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790588"
---
# <a name="supported-platforms"></a>Plataformas compatíveis

Atualmente, o MSIX tem suporte nas seguintes versões do Windows:

* Windows 10, versão 1709 e posterior.
* Windows Server 2019 LTSC e posterior.
* Windows Enterprise 2019 LTSC e posterior.

Para obter mais detalhes sobre o suporte ao ciclo de vida do Windows, como datas de fim de serviço, visite a [folha de fatos do ciclo de vida do Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet)

Este artigo descreve como os principais recursos do MSIX têm suporte nessas versões do Windows.

> [!NOTE]
> O Windows Server 2019 LTSC e o Windows Enterprise 2019 LTSC exigem que o aplicativo **instalador de aplicativo** dê suporte ao clique duas vezes em instalar ou instalar diretamente de um site para. msix,. msixbundle,. Appx ou. appxbundle. Sem o aplicativo, os pacotes podem ser instalados por meio do PowerShell, API ou usam um produto de gerenciamento de sistemas com suporte. Para obter mais considerações sobre o Windows Server 2019 LTSC, consulte [Este artigo](msix-server-2019.md).

> [!NOTE]
> Para versões do Windows anteriores ao Windows 10 versão 1709, use o [MSIX Core](msix-core/msixcore.md) para instalar pacotes MSIX.

## <a name="msix-feature-support"></a>Suporte ao recurso do MSIX

A tabela a seguir mostra quais recursos e cenários do MSIX têm suporte em diferentes versões do Windows.

> [!div class="mx-tableFixed"]
| Recursos | 1.709 | 1803 | 1809 | 1903 | 1909 | 2004| Windows Server 2019 LTSC | Windows Enterprise 2019 LTSC|
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| [Permitir elevação](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| [Suporte ao arquivo do instalador do aplicativo](app-installer/installing-windows10-apps-web.md)| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| [Adiar sinalizador de registro](desktop/managing-your-msix-deployment-update.md) |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |
| [Forçar atualização de qualquer downgrade de versão](desktop/managing-your-msix-deployment-targetdevices.md) |  :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Forçar provisionamento |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |
| Identidade para aplicativos de desktop empacotados | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| [Pacotes de modificação](modification-packages.md) | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Instalação e desinstalação do MSIX nativo | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark: |
| [PSF (Package Support Framework)](psf/package-support-framework-overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:|  :heavy_check_mark: | :heavy_check_mark:|  
| [Serviços Windows](packaging-tool/convert-an-installer-with-services.md) | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x:| :x: | 
| [Imposição de integridade do pacote para pacotes que não são de armazenamento](package/signing-package-overview.md#package-integrity-enforcement) | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x:| :x: | 
## <a name="package-format-support"></a>Suporte a formato de pacote

A tabela a seguir mostra quais formatos de pacote têm suporte em diferentes versões do Windows 10.

| Formato do pacote | 1.709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .msixbundle| :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:|
| .appx | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .appxbundle | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## <a name="microsoft-store"></a>Microsoft Store

A tabela a seguir mostra quais recursos de Microsoft Store têm suporte em diferentes versões do Windows 10.

| Recursos | 1.709 | 1803 | 1809 | 1903 | 1909 | 2004
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

### <a name="non-windows-platform"></a>Plataforma não Windows
O [SDK do MSIX](https://github.com/Microsoft/msix-packaging) é um projeto de software livre que permite aos desenvolvedores usar o formato de pacote MSIX universalmente em todas as plataformas. O SDK pode ser usado por qualquer aplicativo cliente de plataforma cruzada que permita que terceiros criem plug-ins ou extensões. Os desenvolvedores de aplicativos cliente podem usar o modelo de extensão de aplicativo que está disponível na plataforma Windows 10 e usar o SDK do MSIX em plataformas que não são do Windows 10, como macOS, iOS, Android e Linux. 
