---
title: Documentação do arquivo do instalador do aplicativo relacionado
description: Este artigo fornece links para a documentação sobre o esquema de arquivo do instalador de aplicativos e as APIs relacionadas fornecidas pelo SDK do Windows.
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, UWP, instalador de aplicativos, AppInstaller, Sideload, API, XML, esquema
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 39715076139c502b21074b2d180370745c7194f5
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072617"
---
# <a name="app-installer-file-apis"></a>APIs de arquivo do instalador de aplicativo

As classes [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) e [Package](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) no SDK do Windows fornecem métodos que você pode usar para adicionar ou modificar pacotes por meio de arquivos do instalador de aplicativos ou para recuperar informações sobre aplicativos com uma associação de instalador de aplicativo.

|  Método  |  Descrição | Versão mínima com suporte |
|----------|--------------|-------------------|
|  [PackageManager. AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | Permite que pacotes de aplicativos únicos ou múltiplos sejam instalados com um arquivo. AppInstaller. | Atualização dos criadores de outono do Windows 10 (versão 1709, Build 16299)   |
|  [PackageManager. RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | Permite que pacotes de aplicativos únicos ou múltiplos sejam instalados com um arquivo. AppInstaller. Isso executará um filtro do SmartScreen e a verificação do usuário antes de instalar os pacotes do aplicativo. | Atualização dos criadores de outono do Windows 10 (versão 1709, Build 16299)       |
|  [Package. GetAppInstallerInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | Retorna o local do arquivo XML. AppInstaller. Isso permite que os desenvolvedores de aplicativos recuperem o local do arquivo XML. AppInstaller quando necessário para seu aplicativo. | Windows 10, versão 1809 (build 17763) |
|  [Package. CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | Verifica se há atualizações para o pacote principal do aplicativo listado no arquivo. AppInstaller. Ele permite que o desenvolvedor determine se as atualizações são necessárias devido à política. AppInstaller. Esse método atualmente só funciona para aplicativos instalados por meio de arquivos. AppInstaller. | Windows 10, versão 1809 (build 17763) |

## <a name="app-installer-file-schema"></a>Esquema do arquivo do Instalador de Aplicativo

Para obter mais informações sobre como formatar manualmente um arquivo do instalador de aplicativo, consulte [referência de esquema de arquivo do instalador de aplicativo](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).
