---
Description: Este artigo contém todos os detalhes de que você precisa para gerenciar a implantação de aplicativos MSIX em um ambiente empresarial.  Este artigo destina-se a profissionais corporativos e desenvolvedores de TI.
title: Gerenciamento do MSIX com PowerShell
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix, PowerShell, PSH, PS, PoSh, cmdlets
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 7141207e336347f092f7bb23ae35fbd75464887b
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090434"
---
# <a name="managing-msix-with-powershell"></a>Gerenciamento do MSIX com PowerShell
Este artigo descreve os cmdlets do PowerShell usados para gerenciar os pacotes .appx e .msix.

## <a name="msix-powershell-cmdlets"></a>Cmdlets do PowerShell do MSIX

| Cmdlets do PowerShell | Descrição |
|-------------------|-------------|
| [Add-AppPackage](/powershell/module/appx/add-appxpackage?view=win10-ps) | Esse cmdlet é usado para adicionar um pacote de aplicativo assinado (*.msix, *.appx) a um dispositivo. O cmdlet Add-AppPackage também pode ser usado ao adicionar um aplicativo MSIX que tem uma relação com outro aplicativo MSIX, como: Pacotes Externos, [Pacotes Opcionais](../package/optional-packages.md) e [Pacotes Relacionados](../package/optional-packages.md). |
| [Remove-AppPackage](/powershell/module/appx/remove-appxpackage?view=win10-ps) | Esse cmdlet é usado para remover um pacote de aplicativo assinado (*.msix, *.appx) de um dispositivo. Depois da remoção, o conteúdo da pasta na qual o aplicativo foi instalado é removido, bem como qualquer referência ao aplicativo desinstalado no computador. |
| [Get-AppPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) | Esse cmdlet oferecerá uma lista de todos os pacotes de aplicativo autenticados (*.msix, *.appx) instalados no computador. Um valor pode ser informado para filtrar os resultados. Para criar um retorno filtrado, forneça uma cadeia de caracteres completa ou parcial no parâmetro **-Name** usando * como caractere curinga. |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume?view=win10-ps) | Esse cmdlet fornecerá o volume padrão sendo usado pelos pacotes de aplicativo autenticados (*.msix, *.appx) no computador. O volume padrão é o destino de todas as operações de implantação ou instalação em um computador. Esse volume não pode ser removido da lista de volumes. |
| [Get-AppPackageManifest](/powershell/module/appx/get-appxpackagemanifest?view=win10-ps) | Esse cmdlet retornará um objeto xml de manifesto do pacote de aplicativo (*.msix, *.appx) para o nome completo do pacote de aplicativo autenticado especificado. |