---
title: Problemas de API de arquivo do instalador de aplicativo
description: Este artigo lista problemas conhecidos com o Pacotemanager e as APIs de pacote para o gerenciamento de arquivos do instalador de aplicativos.
ms.date: 2/20/2019
ms.topic: article
keywords: Windows 10, UWP, instalador de aplicativos, AppInstaller, Sideload, API
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ca759a80d443daeb58dd66913be2b2acd9e17eef
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328269"
---
# <a name="app-installer-file-api-issues"></a>Problemas de API de arquivo do instalador de aplicativo

## <a name="javascript-support-for-app-installer-file-apis"></a>Suporte a JavaScript para APIs de arquivo do instalador de aplicativo

As classes [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) e [Package](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package) no SDK do Windows fornecem métodos que você pode usar para adicionar ou modificar pacotes por meio de arquivos do instalador de aplicativos ou para recuperar informações sobre aplicativos com uma associação de instalador de aplicativo. Para obter mais informações, confira [Documentação relacionada](app-installer-documentation.md).

Desses métodos, [PackageManager. AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync), [PackageManager. RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)e [Package. CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync) não têm suporte em JavaScript. No entanto, você pode criar um [componente de Windows Runtime](https://docs.microsoft.com/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript) que chama esses métodos e, em seguida, chamar esse componente de um aplicativo UWP JavaScript.

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
