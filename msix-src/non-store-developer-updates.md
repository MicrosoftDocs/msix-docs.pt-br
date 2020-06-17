---
title: Atualizar, por meio do seu código, aplicativos publicados fora da Store
description: Descreve como os pacotes MSIX enviados fora da loja podem ser atualizados por desenvolvedores em código.
author: Huios
ms.date: 06/12/2020
ms.topic: article
keywords: Windows 10, UWP, pacote do aplicativo, atualização do aplicativo, MSIX, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: e68269c0a17254fd92fb00fad6bcbc1d7a16e588
ms.sourcegitcommit: 6243b7aca6f52f007f4571c835f580f433c31769
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84812778"
---
# <a name="update-non-store-published-app-packages-from-your-code"></a>Atualizar pacotes de aplicativos publicados não armazenados do seu código

Ao enviar seu aplicativo como um MSIX, você pode iniciar de forma programática uma atualização do seu aplicativo. Se você implantar seu aplicativo fora da loja, tudo o que você precisa fazer é verificar o servidor em busca de uma nova versão do seu aplicativo e instalar a nova versão. A forma como você aplica a atualização depende se você está implantando o pacote do aplicativo usando um arquivo do instalador do aplicativo ou não. Para aplicar atualizações do seu código, o pacote do aplicativo deve declarar a `packageManagement` funcionalidade.

Este artigo fornece exemplos que demonstram como declarar a `packageManagement` capacidade no manifesto do pacote e como aplicar uma atualização do seu código. A primeira seção analisa como fazer isso se você estiver usando o arquivo do instalador do aplicativo e a segunda seção é sobre como fazer isso quando **não** estiver usando o arquivo do instalador do aplicativo. A última seção analisa como garantir que seu aplicativo seja reiniciado após a aplicação de uma atualização.

## <a name="add-the-packagemanagement-capability-to-your-package-manifest"></a>Adicionar a capacidade de PackageManagement ao manifesto do pacote

Para usar as `PackageManager` APIs, seu aplicativo deve declarar o `packageManagement` [recurso restrito](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) no [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest).

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

## <a name="updating-packages-deployed-using-an-app-installer-file"></a>Atualizando pacotes implantados usando um arquivo do instalador de aplicativos

Se você estiver implantando seu aplicativo usando o arquivo do instalador do aplicativo, qualquer atualização orientada por código que você executar deverá usar as [APIs do arquivo do instalador do aplicativo](https://docs.microsoft.com/windows/msix/app-installer/app-installer-documentation#app-installer-file-apis). Isso garante que as atualizações regulares do arquivo do instalador de aplicativos continuarão funcionando. Para intiating uma atualização baseada no instalador de aplicativo do seu código, você pode usar [PackageManager. AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync?view=winrt-19041) ou [PackageManager. RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync?view=winrt-19041). Você pode verificar se uma atualização está disponível usando a API [Package. CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync?view=winrt-19041) . Abaixo está o código de exemplo:

```csharp
using Windows.Management.Deployment;

public async void CheckForAppInstallerUpdatesAndLaunchAsync(string targetPackageFullName, PackageVolume packageVolume)
{
    // Get the current app's package for the current user.
    PackageManager pm = new PackageManager();
    Package package = pm.FindPackageForUser(string.Empty, targetPackageFullName);

    PackageUpdateAvailabilityResult result = await package.CheckUpdateAvailabilityAsync();
    switch (result.Availability)
    {
        case PackageUpdateAvailability.Available:
        case PackageUpdateAvailability.Required:
            //Queue up the update and close the current instance
            await pm.AddPackageByAppInstallerFileAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.appinstaller"),
            AddPackageByAppInstallerOptions.ForceApplicationShutdown,
            packageVolume);
            break;
        case PackageUpdateAvailability.NoUpdates:
            // Close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
        case PackageUpdateAvailability.Unknown:
        default:
            // Log and ignore error.
            Logger.Log($"No update information associated with app {targetPackageFullName}");
            // Launch target app and close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
    }
}
```

## <a name="updating-packages-deployed-without-an-app-installer-file"></a>Atualizando pacotes implantados sem um arquivo do instalador de aplicativo


### <a name="check-for-updates-on-your-server"></a>Verificar se há atualizações no servidor

Se você **não** estiver usando o arquivo do instalador do aplicativo para implantar o pacote do aplicativo, a primeira etapa será verificar diretamente se uma nova versão do seu aplicativo está disponível. O exemplo a seguir verifica se a versão do pacote em um servidor é maior que a versão atual do aplicativo (Este exemplo refere-se a um servidor de teste para fins de demonstração).

```csharp
using Windows.Management.Deployment;

//check for an update on my server
private async void CheckUpdate(object sender, TappedRoutedEventArgs e)
{
    WebClient client = new WebClient();
    Stream stream = client.OpenRead("https://trial3.azurewebsites.net/HRApp/Version.txt");
    StreamReader reader = new StreamReader(stream);
    var newVersion = new Version(await reader.ReadToEndAsync());
    Package package = Package.Current;
    PackageVersion packageVersion = package.Id.Version;
    var currentVersion = new Version(string.Format("{0}.{1}.{2}.{3}", packageVersion.Major, packageVersion.Minor, packageVersion.Build, packageVersion.Revision));

    //compare package versions
    if (newVersion.CompareTo(currentVersion) > 0)
    {
        var messageDialog = new MessageDialog("Found an update.");
        messageDialog.Commands.Add(new UICommand(
            "Update",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.Commands.Add(new UICommand(
            "Close",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.DefaultCommandIndex = 0;
        messageDialog.CancelCommandIndex = 1;
        await messageDialog.ShowAsync();
    } else
    {
        var messageDialog = new MessageDialog("Did not find an update.");
        await messageDialog.ShowAsync();
    }
}
```

### <a name="apply-the-update"></a>Aplicar a atualização 

Depois de determinar que uma atualização está disponível, você pode enfileirar para download e instalar usando a API do [AddPackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackageasync?view=winrt-19041) . A atualização será aplicada na próxima vez em que seu aplicativo for desligado. Depois que o aplicativo for reiniciado, a nova versão estará disponível para o usuário. Abaixo está o código de exemplo:

```csharp

// Queue up the update and close the current app instance.
private async void CommandInvokedHandler(IUICommand command)
{
    if (command.Label == "Update")
    {
        PackageManager packagemanager = new PackageManager();
        await packagemanager.AddPackageAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.msix"),
            null,
            AddPackageOptions.ForceApplicationShutdown
        );
    }
}
```
## <a name="automatically-restarting-your-app-after-an-update"></a>Reiniciar automaticamente o aplicativo após uma atualização

Se seu aplicativo for um aplicativo UWP, passar em AddPackageByAppInstallerOptions. ForceApplicationShutdown ou addpackageoptions. ForceTargetAppShutdown ao aplicar uma atualização deve agendar o aplicativo para reiniciar após o desligamento + atualização. Para aplicativos não UWP, você precisa chamar [RegisterApplicationRestart](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions#updates) antes de aplicar a atualização.

Você deve chamar RegisterApplicationRestart antes que seu aplicativo comece a ser desligado. Veja abaixo um exemplo de como fazer isso usando os serviços de interoperabilidade para chamar o método nativo em C#:

```csharp
 // Register the active instance of an application for restart in your Update method
 uint res = RelaunchHelper.RegisterApplicationRestart(null, RelaunchHelper.RestartFlags.NONE);
```

Um exemplo da classe auxiliar para chamar o método RegisterApplicationRestart nativo em C#:

```csharp
using System;
using System.Runtime.InteropServices;

namespace MyEmployees.Helpers
{
    class RelaunchHelper
    {
        #region Restart Manager Methods
        /// <summary>
        /// Registers the active instance of an application for restart.
        /// </summary>
        /// <param name="pwzCommandLine">
        /// A pointer to a Unicode string that specifies the command-line arguments for the application when it is restarted.
        /// The maximum size of the command line that you can specify is RESTART_MAX_CMD_LINE characters. Do not include the name of the executable
        /// in the command line; this function adds it for you.
        /// If this parameter is NULL or an empty string, the previously registered command line is removed. If the argument contains spaces,
        /// use quotes around the argument.
        /// </param>
        /// <param name="dwFlags">One of the options specified in RestartFlags</param>
        /// <returns>
        /// This function returns S_OK on success or one of the following error codes:
        /// E_FAIL for internal error.
        /// E_INVALIDARG if rhe specified command line is too long.
        /// </returns>
        [DllImport("kernel32.dll", CharSet = CharSet.Unicode)]
        internal static extern uint RegisterApplicationRestart(string pwzCommandLine, RestartFlags dwFlags);
        #endregion Restart Manager Methods

        #region Restart Manager Enums
        /// <summary>
        /// Flags for the RegisterApplicationRestart function
        /// </summary>
        [Flags]
        internal enum RestartFlags
        {
            /// <summary>None of the options below.</summary>
            NONE = 0,

            /// <summary>Do not restart the process if it terminates due to an unhandled exception.</summary>
            RESTART_NO_CRASH = 1,
            /// <summary>Do not restart the process if it terminates due to the application not responding.</summary>
            RESTART_NO_HANG = 2,
            /// <summary>Do not restart the process if it terminates due to the installation of an update.</summary>
            RESTART_NO_PATCH = 4,
            /// <summary>Do not restart the process if the computer is restarted as the result of an update.</summary>
            RESTART_NO_REBOOT = 8
        }
        #endregion Restart Manager Enums

    }
}
```
