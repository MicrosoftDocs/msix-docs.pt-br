---
title: Pacotes de recursos
description: Este artigo descreve os pacotes de recursos e como os desenvolvedores podem usá-los em seu código.
ms.date: 02/06/2020
author: dianmsft
ms.topic: article
ms.author: diahar
keywords: Windows 10, msix, UWP, pacotes opcionais, conjunto relacionado, extensão do pacote, Visual Studio
ms.localizationpriority: medium
ms.openlocfilehash: 1900e748075339ad6843f13a6c145147422e2db0
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073762"
---
# <a name="resource-packages"></a>Pacotes de recursos 
Os pacotes de recursos oferecem uma ótima maneira de reduzir a superfície de disco dos usuários ao segmentar a linguagem ou o ativo específico de escala em pacotes separados que são baixados automaticamente pelo Windows, dependendo da configuração do computador dos usuários. Se o usuário adicionar um novo idioma à lista de idiomas do sistema operacional na **região &** configurações de idioma ou alterar a configuração de exibição, em uma atualização de repositório automática, o sistema operacional buscará pacotes de recursos aplicáveis para todos os aplicativos instalados no dispositivo.

Na API SDK do Windows 10.0.17095.0 [AddResourcePackageAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.PackageCatalog) permite que os desenvolvedores instalem um pacote de recursos para um aplicativo sob demanda. 


## <a name="how-to-use-the-addresourcepackageasync-api"></a>Como usar a API AddResourcePackageAsync 
- AddResourcePackageAsync usa o **PackageFamilyName** do aplicativo e a ID do recurso que identifica exclusivamente o pacote de recursos que está tentando baixar. A ID do recurso corresponde ao elemento **ResourceId** no **AppxManifest. xml** do pacote de recursos.

- O aplicativo deve ser reiniciado para obter uma exibição mesclada de todos os recursos que estão disponíveis para o aplicativo. A API oferece a opção **ForceTargetApplicationShutdown** que pode passar para reiniciar o aplicativo.

- Se houver uma atualização disponível para o aplicativo quando o pacote de recursos estiver sendo baixado, a API falhará, pois a versão mais antiga do aplicativo não estará mais disponível. Ao passar o sinalizador **ApplyUpdateIfAvailable** , a API atualizará o aplicativo, bem como obterá o pacote de recursos solicitado como parte do mesmo download. 

- A API retorna um progresso para o download que pode ser exibido no aplicativo. Observe também que, para aplicativos publicados na loja, a aquisição do pacote de recursos aparece na página Microsoft Store **downloads e atualizações** e aparece como uma atualização para seu aplicativo. O usuário final pode optar por pausar ou cancelar o download do pacote de recursos do Microsoft Store. 

{1&gt;Exemplo&lt;1} 
```
 private async void DownloadGermanResourcePackage(object sender, RoutedEventArgs e)
{            
    // ResourceId that is unique to the resource package
    string resourceId = "split.language-de";
    // Warn user that application will need to restart.
    // To take update if available pass in the ApplyUpdateIfAvailable flag
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    PackageCatalogAddResourcePackageResult result = await packageCatalog.AddResourcePackageAsync("29270depappf.CaffeMacchiato_gah1vdar1nn7a", resourceId, 
        AddResourcePackageOptions.ApplyUpdateIfAvailable | AddResourcePackageOptions.ForceTargetApplicationShutdown)
        .AsTask<PackageCatalogAddResourcePackageResult, PackageInstallProgress>(new Progress
        (progress =>;
        {
                // Draw progress
        }));

        if (result.ExtendedError != null)
        {
                //Display error or retry if needed
        }
}
```
## <a name="validation"></a>Validação
 Para a validação local, os desenvolvedores podem criar um. msixbundle ou. appxbundle e instalar a partir da unidade local, do compartilhamento de rede ou do servidor da Web. Quando o aplicativo chamar a API AddResourcePackageAsync, o Windows adquirirá o pacote de recursos do local onde o aplicativo original foi instalado. Isso torna a validação simples. Quando isso funcionar, o aplicativo estará pronto para ser implantado. 

## <a name="removing-resource-packages"></a>Removendo pacotes de recursos 
O aplicativo pode optar por remover para remover os pacotes de recursos baixados por meio da [API RemoveResourcePackageAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.PackageCatalog). No entanto, os pacotes de recursos que são aplicáveis para o usuário ou para o dispositivo como um todo não podem ser desinstalados. 

{1&gt;Exemplo&lt;1} 
```
 private async void RemoveResourcePackage(object sender, RoutedEventArgs e)
{            
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    List resourcePackagesToRemove = new List();
    resourcePackagesToRemove.Add("split.language-de");
    //Warn user that application will be restarted
    var removePackageResult = await packageCatalog.RemoveResourcePackagesAsync(resourcePackagesToRemove);
    if (removePackageResult.ExtendedError != null)
    {
        // display error
    }
}
```
