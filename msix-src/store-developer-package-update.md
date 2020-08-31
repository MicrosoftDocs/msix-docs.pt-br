---
title: Atualizar, por meio do seu código, aplicativos publicados na Store
description: Descreve como os pacotes do MSIX podem ser atualizados por desenvolvedores no código.
author: Huios
ms.date: 01/24/2020
ms.topic: article
keywords: Windows 10, UWP, pacote do aplicativo, atualização do aplicativo, MSIX, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 381c3f06ebc0d9ea3093e3dab48d29eeb0c2533a
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090661"
---
# <a name="update-store-published-apps-from-your-code"></a>Atualizar, por meio do seu código, aplicativos publicados na Store

A partir do Windows 10, versão 1607 (Build 14393), o Windows 10 permite que os desenvolvedores tomem garantias mais fortes em relação às atualizações de aplicativos da loja. Fazer isso requer algumas APIs simples, cria uma experiência de usuário consistente e previsível e permite que os desenvolvedores se concentrem no que fazem melhor, ao mesmo tempo em que permitem que o Windows faça o trabalho pesado.

Há duas maneiras fundamentais pelas quais as atualizações de aplicativo podem ser gerenciadas. Em ambos os casos, o resultado líquido para esses métodos é o mesmo-a atualização é aplicada. No entanto, em um caso, você pode optar por permitir que o sistema faça todo o trabalho, enquanto, em outros casos, talvez você queira ter um nível mais profundo de controle sobre a experiência do usuário.

## <a name="simple-updates"></a>Atualizações simples

A primeira e mais importante é a chamada de API muito simples que diz ao sistema para verificar se há atualizações, baixá-las e solicitar permissão do usuário para instalá-las. Você começará usando a classe [StoreContext](/uwp/api/Windows.Services.Store.StoreContext) para obter objetos [StorePackageUpdate](/uwp/api/Windows.Services.Store.StorePackageUpdate) , baixá-los e instalá-los.

```csharp
using Windows.Services.Store;

private async void GetEasyUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation = 
            updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);
        StorePackageUpdateResult result = await downloadOperation.AsTask();
    }
}
```

Neste ponto, o usuário tem duas opções que podem escolher: aplicar a atualização agora ou adiar a atualização. Qualquer que seja a opção que o usuário faça será retornada por meio do `StorePackageUpdateResult` objeto, permitindo que os desenvolvedores realizem ações adicionais, como fechar o aplicativo se a atualização for necessária para continuar ou simplesmente tentar novamente mais tarde.

## <a name="fine-controlled-updates"></a>Atualizações controladas corretamente

Para os desenvolvedores que desejam ter uma experiência completamente personalizada, são fornecidas APIs adicionais que permitem mais controle sobre o processo de atualização. A plataforma permite que você faça o seguinte:

* Obter eventos de progresso em um download de pacote individual ou em toda a atualização.
* Aplique atualizações na conveniência do usuário e do aplicativo em vez de uma ou outra.

Os desenvolvedores podem baixar atualizações em segundo plano (enquanto o aplicativo estiver em uso) e solicitar que o usuário instale atualizações, se elas recusarem, você poderá simplesmente desabilitar os recursos afetados pela atualização, se escolher.

### <a name="download-updates"></a>Baixar atualizações

```csharp
private async void DownloadUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
            updateManager.RequestDownloadStorePackageUpdatesAsync(updates);

        downloadOperation.Progress = async (asyncInfo, progress) =>
        {
            // Show progress UI
        };

        StorePackageUpdateResult result = await downloadOperation.AsTask();
        if (result.OverallState == StorePackageUpdateState.Completed)
        {
            // Update was downloaded, add logic to request install
        }
    }
}
```

### <a name="install-updates"></a>Instalar as atualizações

```csharp
private async void InstallUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();    

    // Save app state here

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    StorePackageUpdateResult result = await installOperation.AsTask();

    // Under normal circumstances, app will terminate here

    // Handle error cases here using StorePackageUpdateResult from above
}
```

## <a name="making-updates-mandatory"></a>Fazendo atualizações obrigatórias

Em alguns casos, pode ser realmente desejável ter uma atualização que deve ser instalada no dispositivo de um usuário, tornando-a realmente obrigatória (por exemplo, uma correção crítica para um aplicativo que não pode esperar). Nesses casos, há medidas adicionais que você pode executar para tornar a atualização obrigatória.

1. Implemente a lógica de atualização obrigatória em seu código de aplicativo (precisaria ser feita antes da atualização obrigatória).
2. Durante o envio para o centro de desenvolvimento, certifique-se de que a caixa **tornar esta atualização obrigatória** esteja selecionada.

### <a name="implementing-app-code"></a>Implementando o código do aplicativo

Para aproveitar ao máximo as atualizações obrigatórias, você precisará fazer algumas pequenas modificações no código acima. Você precisará usar o [objeto StorePackageUpdate](/uwp/api/Windows.Services.Store.StorePackageUpdate) para determinar se a atualização é obrigatória.

```csharp
 private async bool CheckForMandatoryUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        foreach (StorePackageUpdate u in updates)
        {
            if (u.Mandatory)
                return true;
        }
    }
    return false;
}
```

Em seguida, você precisará criar uma caixa de diálogo personalizada no aplicativo para informar ao usuário que há uma atualização obrigatória e que ela deve instalá-la para continuar o uso completo do aplicativo. Se o usuário recusar a atualização, o aplicativo poderá degradar a funcionalidade (por exemplo, impedir o acesso online) ou terminar completamente (por exemplo, jogos somente online).

### <a name="partner-center"></a>Partner Center

Para garantir que o [StorePackageUpdate](/uwp/api/Windows.Services.Store.StorePackageUpdate) mostra true para uma atualização obrigatória, você precisará marcar a atualização como obrigatória no Partner Center na página **pacotes** .

Algumas coisas a serem observadas:

* Se um dispositivo voltar a ficar online depois que uma atualização obrigatória tiver sido substituída por outra atualização não obrigatória, a atualização não obrigatória ainda aparecerá no dispositivo como obrigatória, dada a atualização perdida antes de ser obrigatória.
* As atualizações controladas pelo desenvolvedor e as atualizações obrigatórias estão limitadas à loja no momento.