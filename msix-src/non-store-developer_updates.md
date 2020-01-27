---
title: Atualizar aplicativos publicados não armazenados do seu código
description: Descreve como os pacotes MSIX enviados fora da loja podem ser atualizados por desenvolvedores em código.
author: Huios
ms.date: 01/24/2020
ms.topic: article
keywords: Windows 10, UWP, pacote do aplicativo, atualização do aplicativo, MSIX, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 3dea6cf0f60846fd35190c642a1686134895d364
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726606"
---
# <a name="update-non-store-published-apps-from-your-code"></a>Atualizar aplicativos publicados não armazenados do seu código

Ao enviar seu aplicativo como um MSIX, você pode iniciar de forma programática uma atualização do seu aplicativo. Se você implantar seu aplicativo fora da loja, tudo o que você precisa fazer é verificar o servidor em busca de uma nova versão do seu aplicativo e instalar a nova versão. Você pode atualizar para uma nova versão tirando proveito do método [PackageManager. UpdatePackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.updatepackageasync) . Isso requer que seu aplicativo declare o recurso de `packageManagement`.

Este artigo fornece exemplos que demonstram como declarar o recurso de `packageManagement` no manifesto do pacote e como usar o método [PackageManager. UpdatePackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.updatepackageasync) para verificar e instalar uma atualização.

## <a name="add-the-packagemanagement-capability-to-your-package-manifest"></a>Adicionar a capacidade de PackageManagement ao manifesto do pacote

Para usar as APIs de `PackageManager`, seu aplicativo deve declarar a [funcionalidade `packageManagement` restrita](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) no [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest).

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

## <a name="check-for-updates-on-your-server"></a>Verificar se há atualizações no servidor

A primeira etapa é verificar se uma nova versão do seu aplicativo está disponível. O exemplo a seguir verifica se a versão do pacote em um servidor é maior que a versão atual do aplicativo (Este exemplo refere-se a um servidor de teste para fins de demonstração).

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

## <a name="download-an-update"></a>Baixar uma atualização

Depois de determinar que uma atualização está disponível, você pode enfileirar-a para download e instalação. A atualização será aplicada na próxima vez em que seu aplicativo for desligado. Depois que o aplicativo for reiniciado, a nova versão estará disponível para o usuário.

```csharp

// Queue up the update and close the current app instance.
private async void CommandInvokedHandler(IUICommand command)
{
    if (command.Label == "Update")
    {
        PackageManager packagemanager = new PackageManager();
        await packagemanager.UpdatePackageAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.msix"),
            null,
            DeploymentOptions.ForceApplicationShutdown
        );
    }
}
```
