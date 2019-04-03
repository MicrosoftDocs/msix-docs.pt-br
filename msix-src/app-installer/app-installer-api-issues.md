---
author: joshusto
title: Problemas de API do arquivo do instalador do aplicativo
description: Listas de problemas conhecidos com as APIs de arquivos do instalador do aplicativo.
ms.author: joshusto
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, uwp, o instalador do aplicativo, AppInstaller, fazer sideload de API
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: be13529ecbb1845447719c7951b77ca66287ffc0
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900418"
---
# <a name="app-installer-file-api-issues"></a>Problemas de API do arquivo do instalador do aplicativo

## <a name="javascript-support-for-app-installer-file-apis"></a>Suporte a JavaScript para APIs de arquivo de instalador de aplicativo

O [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) e [pacote](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) classes no SDK do Windows fornecem métodos que você pode usar para adicionar ou modificar os pacotes por meio de arquivos do instalador do aplicativo ou para recuperar informações sobre os aplicativos com um instalador de aplicativo associação. Para obter mais informações, consulte [documentação relacionada](app-installer-documentation.md).

Esses métodos, [PackageManager.AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync), [PackageManager.RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync), e [ Package.CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync) não têm suporte em JavaScript. No entanto, você pode criar uma [componente de tempo de execução do Windows](https://docs.microsoft.com/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript) que chama esses métodos e, em seguida, chamar este componente de um aplicativo JavaScript UWP.

Aqui está um exemplo.

```csharp
namespace CSRuntimeComponent
{
    public sealed class UpdateAvailabilityChecker
    {
        public static IAsyncOperation<PackageUpdateAvailability> CheckForUpdatesAsync()
        {
            return AsyncInfo.Run<PackageUpdateAvailability>((result) => Task.Run<PackageUpdateAvailability>(async () =>
            {
                PackageManager pm = new PackageManager();
                Package currentPackage = pm.FindPackageForUser(string.Empty, Package.Current.Id.FullName);
                PackageUpdateAvailabilityResult apiResult = await currentPackage.CheckUpdateAvailabilityAsync();

                if (apiResult.Availability == PackageUpdateAvailability.Error)
                {
                    Logger.Error($"Error occurred, extended code: {apiResult.ExtendedError}");
                }

                return apiResult.Availability;
            }));
        }
    }
}
```

```javascript
window.onload = function () {
    document.getElementById('mainButton').onclick = function (evt) {
        CSRuntimeComponent.UpdateAvailabilityChecker.checkForUpdatesAsync().done(function (result) {
            document.getElementById("resultLabel").innerHTML = "Update availability result:" + result;
        });
    }
}
```
