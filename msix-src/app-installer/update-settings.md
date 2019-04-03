---
title: Configurações de atualização de arquivo do instalador de aplicativo
description: Saiba como configurar atualizações de aplicativo usando o arquivo do instalador do aplicativo.
author: mcleanbyron
ms.author: mcleans
ms.date: 12/12/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fd234323185200895a6eaf276988fbe7a5c363b3
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900348"
---
# <a name="configure-update-settings-in-the-app-installer-file"></a>Definir configurações de atualização no arquivo de instalador de aplicativo

Conforme mencionado na [visão geral de arquivo do instalador do aplicativo](app-installer-file-overview.md), você pode configurar o comportamento de atualização do aplicativo no arquivo de instalador de aplicativo. Este artigo explora as opções de atualização e seus respectivas prós e contras.

Você pode configurar o comportamento de atualização do aplicativo usando o [UpdateSettings](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings) elemento. Aqui, exploraremos as opções de atualização e seus respectivas prós e contras.

Em resumo, você pode optar por verificar se há atualizações de duas maneiras diferentes:
1. Independentemente do usuário iniciar o aplicativo.
2. Somente quando o usuário inicia o aplicativo.

Além disso, você pode optar por aplicar as atualizações de duas maneiras diferentes:
1. Por informando ao usuário com um prompt.
2. Modo silencioso, sem informando ao usuário.

Por fim, quando você informar o usuário de uma atualização, você pode forçar para que tenham a atualização antes de permitir iniciar o aplicativo, ou você pode permitir que eles possam iniciar o aplicativo e aplicar a atualização em um momento oportuno.

Especificamente, a sintaxe que está disponível para você é o seguinte:

- O [UpdateSettings](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings) elemento pode ter os seguintes elementos:

    - **OnLaunch**: Verifica se há atualizações na inicialização. Esse tipo de atualização pode mostrar a interface do usuário e tem os seguintes atributos:

        - **ShowPrompt**: Um booliano que determina se a interface do usuário será mostrada ao usuário.

        - **UpdateBlocksActivation**: Um booliano que determina se a interface do usuário mostrado ao usuário permite que o usuário iniciar o aplicativo sem realizar a atualização, ou se o usuário deve executar a atualização antes de iniciar o aplicativo. Esse atributo pode ser definido como "true" apenas se **ShowPrompt** é definido como "true". **UpdateBlocksActivation**= "true" significa que a interface do usuário, o usuário verá, permite que o usuário fazer a atualização ou fechar o aplicativo. **UpdateBlocksActivation**= "false" significa a interface do usuário, o usuário verá, permite que o usuário fazer a atualização ou iniciar o aplicativo sem a atualização. No último caso, a atualização será aplicada silenciosamente em um momento oportuno. Disponível no Windows 10, versão 1809 e posterior.

        > [!NOTE]
        > ShowPrompt precisa ser definido como true se UpdateBlocksActivation é definido como true.

        - **HoursBetweenUpdateChecks**: Um inteiro que indica a frequência (em quantas horas) o sistema verificará se há atualizações para o aplicativo. "0" a "255" inclusivo. O valor padrão é 24 (se esse valor não for especificado). Por exemplo se HoursBetweenUpdateChecks = 3, em seguida, quando o usuário inicia o aplicativo, se o sistema não foi verificado para atualizações de dentro das últimas 3 horas, ele verificará se há atualizações agora.  

    - **AutomaticBackgroundTask**: Verifica se há atualizações em segundo plano, independentemente se o usuário iniciou o aplicativo a cada 8 horas. Esse tipo de atualização não é possível mostrar a interface do usuário.

    - **ForceUpdateFromAnyVersion**: Permite que o aplicativo para atualizar da versão x para versão x + + ou fazer o downgrade da versão x para versão x-. Sem esse elemento, o aplicativo só pode mover para uma versão superior.
