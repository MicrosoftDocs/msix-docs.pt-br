---
title: Automatizar a conversão dos instaladores do Windows para pacotes MSIX
description: Automatizar a conversão de instaladores do windows existente usando a interface de linha de comando para gerar pacotes msix
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4408381a0ebbcc7fbdad7c517c1b64bbec2254da
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900168"
---
# <a name="automate-conversion-of-windows-installers-to-msix-packages"></a>Automatizar a conversão dos instaladores do Windows para pacotes MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>

Ferramenta de empacotamento MSIX dá suporte à interface de linha de comando para a criação de pacotes de aplicativos MSIX que permite ao usuário automatizar o reempacotamento e fazendo conversões em massa com um simple script do PowerShell.

Abaixo está um script do powershell simples que cria modelos correspondentes para cada instalador em um caminho de pasta usando um modelo de conversão de exemplo e passa os modelos para a ferramenta de empacotamento MSIX para criar pacotes de MSIX.


```ps1
$root = "C:\Installers\"
$ConversionTemplate = $root + "\SampleTemplate.xml"
$MSIXSaveLocation = "C:\MSIX\Converted"

get-childitem $root -recurse | where {$_.extension -eq ".msi"} | % {
  
    $Installerpath = $_.FullName
    $filename = $_.BaseName
    $XML_Path = $root + $filename + "ConversionTemplate.xml"

    [xml]$XmlDocument = Get-Content $ConversionTemplate
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Path = $Installerpath
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Arguments = "/qb"
    $XmlDocument.MSIXPackagingToolTemplate.Installer.InstallLocation = "C:\Program Files (x86)\"

    $XmlDocument.MSIXPackagingToolTemplate.SaveLocation.Path = $MSIXSaveLocation 
    
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherName = "CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" 
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ExecutableName = $filename +".exe"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.DisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.Description = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ID = $filename +"1"

    $xmldocument.Save($XML_Path)

    MsixPackagingTool.exe create-package --template $XML_Path

}
```

