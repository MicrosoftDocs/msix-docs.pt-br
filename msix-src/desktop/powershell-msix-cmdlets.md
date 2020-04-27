---
Description: Este artigo contém todos os detalhes de que você precisa para gerenciar a implantação de aplicativos MSIX em um ambiente empresarial.  Este artigo destina-se a profissionais corporativos e desenvolvedores de TI.
title: Gerenciamento do MSIX com PowerShell
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix, PowerShell, PSH, PS, PoSh, cmdlets
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: b5c4366ac7e22fc67fb3314737c9f59f4a057aca
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "77074042"
---
# <a name="managing-msix-with-powershell"></a>Gerenciamento do MSIX com PowerShell
Este artigo descreve os cmdlets do PowerShell usados para gerenciar os pacotes .appx e .msix.

## <a name="msix-powershell-cmdlets"></a>Cmdlets do PowerShell do MSIX

| Cmdlets do PowerShell | Descrição |
|-------------------|-------------|
| [Add-AppPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) | Esse cmdlet é usado para adicionar um pacote de aplicativo assinado (*.msix, *.appx) a um dispositivo. O cmdlet Add-AppPackage também pode ser usado ao adicionar um aplicativo MSIX que tem uma relação com outro aplicativo MSIX, como: Pacotes Externos, [Pacotes Opcionais](https://docs.microsoft.com/windows/msix/package/optional-packages) e [Pacotes Relacionados](https://docs.microsoft.com/windows/msix/package/optional-packages). |
| [Remove-AppPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps) | Esse cmdlet é usado para remover um pacote de aplicativo assinado (*.msix, *.appx) de um dispositivo. Depois da remoção, o conteúdo da pasta na qual o aplicativo foi instalado é removido, bem como qualquer referência ao aplicativo desinstalado no computador. |
| [Get-AppPackage](https://docs.microsoft.com/powershell/module/appx/get-appxpackage?view=win10-ps) | Esse cmdlet oferecerá uma lista de todos os pacotes de aplicativo autenticados (*.msix, *.appx) instalados no computador. Um valor pode ser informado para filtrar os resultados. Para criar um retorno filtrado, forneça uma cadeia de caracteres completa ou parcial no parâmetro **-Name** usando * como caractere curinga. |
| [Get-AppxDefaultVolume](https://docs.microsoft.com/powershell/module/appx/get-appxdefaultvolume?view=win10-ps) | Esse cmdlet fornecerá o volume padrão sendo usado pelos pacotes de aplicativo autenticados (*.msix, *.appx) no computador. O volume padrão é o destino de todas as operações de implantação ou instalação em um computador. Esse volume não pode ser removido da lista de volumes. |
| [Get-AppPackageManifest](https://docs.microsoft.com/powershell/module/appx/get-appxpackagemanifest?view=win10-ps) | Esse cmdlet retornará um objeto xml de manifesto do pacote de aplicativo (*.msix, *.appx) para o nome completo do pacote de aplicativo autenticado especificado. |
