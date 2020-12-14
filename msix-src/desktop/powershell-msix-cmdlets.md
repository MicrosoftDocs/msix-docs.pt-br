---
description: Este artigo contém todos os detalhes de que você precisa para gerenciar a implantação de aplicativos MSIX em um ambiente empresarial.  Este artigo destina-se a profissionais corporativos e desenvolvedores de TI.
title: Gerenciamento do MSIX com PowerShell
ms.date: 12/4/2020
ms.topic: article
keywords: windows 10, implantação, msix, PowerShell, PSH, PS, PoSh, cmdlets
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: e90acc3aa30f689943d16c014db87322af3e8812
ms.sourcegitcommit: de0c711ce28851ef6976a71dd1317291f278b4d1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96612104"
---
# <a name="managing-msix-with-powershell"></a>Gerenciamento do MSIX com PowerShell
Este artigo descreve os cmdlets do PowerShell usados para gerenciar os pacotes .appx e .msix.

## <a name="msix-powershell-cmdlets"></a>Cmdlets do PowerShell do MSIX

| Cmdlets do PowerShell | Descrição |
|-------------------|-------------|
| [Add-AppPackage](/powershell/module/appx/add-appxpackage) | Esse cmdlet é usado para adicionar um pacote de aplicativo assinado (*.msix, *.appx) a um dispositivo. O cmdlet Add-AppPackage também pode ser usado ao adicionar um aplicativo MSIX que tem uma relação com outro aplicativo MSIX, como: Pacotes Externos, [Pacotes Opcionais](../package/optional-packages.md) e [Pacotes Relacionados](../package/optional-packages.md). |
| [Remove-AppPackage](/powershell/module/appx/remove-appxpackage) | Esse cmdlet é usado para remover um pacote de aplicativo assinado (*.msix, *.appx) de um dispositivo. Depois da remoção, o conteúdo da pasta na qual o aplicativo foi instalado é removido, bem como qualquer referência ao aplicativo desinstalado no computador. |
| [Get-AppPackage](/powershell/module/appx/get-appxpackage) | Esse cmdlet oferecerá uma lista de todos os pacotes de aplicativo autenticados (*.msix, *.appx) instalados no computador. Um valor pode ser informado para filtrar os resultados. Para criar um retorno filtrado, forneça uma cadeia de caracteres completa ou parcial no parâmetro **-Name** usando * como caractere curinga. |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume) | Esse cmdlet fornecerá o volume padrão sendo usado pelos pacotes de aplicativo autenticados (*.msix, *.appx) no computador. O volume padrão é o destino de todas as operações de implantação ou instalação em um computador. Esse volume não pode ser removido da lista de volumes. |
| [Get-AppPackageManifest](/powershell/module/appx/get-appxpackagemanifest) | Esse cmdlet retornará um objeto xml de manifesto do pacote de aplicativo (*.msix, *.appx) para o nome completo do pacote de aplicativo autenticado especificado. |
| [Reset-AppxPackage](/powershell/module/appx/reset-appxpackage) | Esse cmdlet redefinirá o aplicativo instalado para as configurações originais. |
| [Get-AppxVolume](/powershell/module/appx/get-appxvolume) | Esse cmdlet retornará uma lista de objetos AppxVolume que são conhecidos pelo computador. |
| [Add-AppxVolume](/powershell/module/appx/add-appxvolume) | Esse cmdlet adicionará um novo AppxVolume para que o Gerenciador de Pacotes publique anúncios. |
| [Remove-AppxVolume](/powershell/module/appx/remove-appxvolume) | Esse cmdlet removerá um AppxVolume existente do dispositivo. |
| [Mount-AppxVolume](/powershell/module/appx/mount-appxvolume) | Esse cmdlet montará um AppxVolume, permitindo que todos os aplicativos implantados no destino se tornem acessíveis. |
| [Dismount-AppxVolume](/powershell/module/appx/dismount-appxvolume) | Esse cmdlet desmontará um AppxVolume, removendo o acesso a aplicativos que são implantados no destino. |
| [Move-AppxPackage](/powershell/module/appx/move-appxpackage) | Esse cmdlet moverá um pacote de aplicativo do Windows do local atual para outro AppxVolume montado. |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume) | Esse cmdlet obterá o AppxVolume padrão usado como o destino de todas as operações de implantação para o dispositivo. |
| [Set-AppxDefaultVolume](/powershell/module/appx/set-appxdefaultvolume) | Esse cmdlet definirá outro AppxVolume montado como o novo destino padrão para todas as operações de implantação no dispositivo. |
| [Invoke-CommandInDesktopPackage](/powershell/module/appx/invoke-commandindesktoppackage) | Esse cmdlet possibilita executar comandos na bolha do Pacote de Aplicativo do Windows. |
