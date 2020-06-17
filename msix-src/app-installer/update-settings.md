---
title: Configurações de atualização de arquivo do instalador de aplicativo
description: Este artigo descreve as opções de como configurar o comportamento de atualizações de aplicativo usando o arquivo do instalador do aplicativo.
ms.date: 06/12/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 10a8c443097f15ab18144198134d1f0fcedb939d
ms.sourcegitcommit: 6243b7aca6f52f007f4571c835f580f433c31769
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84812750"
---
# <a name="configure-update-settings-in-the-app-installer-file"></a>Definir configurações de atualização no arquivo do Instalador de Aplicativo

Conforme mencionado na [visão geral do arquivo do instalador do aplicativo](app-installer-file-overview.md), você pode configurar o comportamento de atualização do aplicativo no arquivo do instalador do aplicativo. Este artigo explora as opções de atualização e suas respectivas compensações.

Você pode configurar o comportamento de atualização do aplicativo usando o elemento [UpdateSettings](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings) . Aqui, exploramos as opções de atualização e suas respectivas compensações.

Em suma, você pode optar por verificar se há atualizações de duas maneiras diferentes:
1. Independentemente do usuário que está iniciando o aplicativo.
2. Somente quando o usuário inicia o aplicativo.

Além disso, você pode optar por aplicar atualizações de duas maneiras diferentes:
1. Informando ao usuário um prompt.
2. Silenciosamente, sem informar o usuário.

Por fim, ao informar ao usuário uma atualização, você pode forçá-los a fazer a atualização antes de permitir que eles iniciem o aplicativo ou você pode permitir que eles iniciem o aplicativo e apliquem a atualização em um horário Opportune.


O elemento [UpdateSettings](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings) pode ter os seguintes elementos filho:

| Configuração de atualização de arquivo do instalador de aplicativo | Versão mínima do Windows 10
|------------------|--------------------|
|  OnLaunch| 1.709                |
|  HoursBetweenUpdateChecks| 1803                |
| AutomaticBackgroundTask | 1803 |
| UpdateBlocksActivation  | 1903 |
|  Exibir | 1903 |
|  ForceUpdateFromAnyVersion | 1903 |

- **OnLaunch**: verifica se há atualizações na inicialização. Esse tipo de atualização pode mostrar a interface do usuário e tem os seguintes atributos:

    - **HoursBetweenUpdateChecks**: um inteiro que indica com que frequência (em quantas horas) o sistema verificará se há atualizações para o aplicativo. "0" a "255" inclusivo. O valor padrão é 24 (se esse valor não for especificado). Por exemplo, se HoursBetweenUpdateChecks = 3, quando o usuário iniciar o aplicativo, se o sistema não tiver verificado se há atualizações nas últimas 3 horas, ele verificará se há atualizações agora.  

     - **Mostrar**: um booliano que determina se a IU será mostrada ao usuário. Esse valor tem suporte no Windows 10, versão 1903 e posterior.

     - **UpdateBlocksActivation**: um booliano que determina se a interface do usuário mostrada para a usuária permite que o usuário inicie o aplicativo sem fazer a atualização ou se o usuário deve fazer a atualização antes de iniciar o aplicativo. Esse atributo pode ser definido como "true" **somente se o** "true" for definido como "verdadeiro". **UpdateBlocksActivation**= "true" significa que a interface do usuário será exibida, permite que o usuário faça a atualização ou feche o aplicativo. **UpdateBlocksActivation**= "false" significa que a interface do usuário será exibida, permite que o usuário faça a atualização ou inicie o aplicativo sem Atualizar. No último caso, a atualização será aplicada silenciosamente em um horário Opportune. Esse valor tem suporte no Windows 10, versão 1903 e posterior.

        > [!NOTE]
        > O @ prompt precisa ser definido como true se UpdateBlocksActivation estiver definido como true.

- **AutomaticBackgroundTask**: verifica se há atualizações em segundo plano a cada 8 horas independentemente de o usuário ter iniciado o aplicativo. Esse tipo de atualização não pode mostrar a interface do usuário.

- **ForceUpdateFromAnyVersion**: permite que o aplicativo atualize da versão x para a versão x + + ou para fazer downgrade da versão x para a versão x--. Sem esse elemento, o aplicativo só pode mudar para uma versão superior.
