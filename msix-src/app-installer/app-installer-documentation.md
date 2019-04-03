---
author: joshusto
title: Documentação relacionada de arquivo de instalador de aplicativo
description: Lista informações sobre o esquema de arquivo do instalador de aplicativos e APIs.
ms.author: joshusto
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, uwp, o instalador do aplicativo, AppInstaller, carregar, API, XML, esquema
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 38a18c50ac1be215819b870215f89b9042d060d8
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900688"
---
# <a name="related-app-installer-file-documentation"></a>Documentação relacionada de arquivo de instalador de aplicativo

## <a name="app-installer-file-apis"></a>APIs de arquivos do instalador de aplicativo

O [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) e [pacote](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) classes no SDK do Windows fornecem métodos que você pode usar para adicionar ou modificar pacotes por meio de arquivos do instalador de aplicativo, ou para recuperar informações sobre os aplicativos com um instalador de aplicativo associação.

|  Método  |  Descrição | Versão mínima suportada |
|----------|--------------|-------------------|
|  [PackageManager.AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | Permite que os pacotes de aplicativo único ou vários ser instalado com um arquivo appinstaller. | Windows 10 Fall Creators Update (versão 1709, build 16299)   |
|  [PackageManager.RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | Permite que os pacotes de aplicativo único ou vários ser instalado com um arquivo appinstaller. Isso executará uma verificação de usuário e de filtro do SmartScreen antes de instalar o pacote de aplicativo (s). | Windows 10 Fall Creators Update (versão 1709, build 16299)       |
|  [Package.GetAppInstallerInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | Retorna o local do arquivo appinstaller xml. Isso permite que os desenvolvedores de aplicativos recuperar o local do arquivo appinstaller xml quando necessário, seu aplicativo. | Windows 10, versão 1809 (build 17763) |
|  [Package.CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | Verifica se há atualizações para o pacote de aplicativo principal listados no arquivo appinstaller. Ele permite que o desenvolvedor determinar se as atualizações são necessárias devido à política de appinstaller. Este método atualmente só funciona para aplicativos instalados por meio de arquivos appinstaller. | Windows 10, versão 1809 (build 17763) |

## <a name="app-installer-file-schema"></a>Esquema de arquivo do instalador de aplicativo

Para obter mais informações sobre como formatar manualmente um arquivo do instalador do aplicativo, consulte [referência de esquema de arquivo de instalador de aplicativo](https://docs.microsoft.com/en-us/uwp/schemas/appinstallerschema/app-installer-file).
