---
title: Arquiteturas de pacote do aplicativo
description: Saiba mais sobre quais arquiteturas de processador você deve usar ao criar seu pacote do aplicativo UWP.
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10, uwp, empacotamento, arquitetura, configuração do pacote
ms.localizationpriority: medium
ms.openlocfilehash: 857c3cd89d17d8a442c1ccbe2897f549d8d7b8d9
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091094"
---
# <a name="app-package-architectures"></a>Arquiteturas de pacote do aplicativo

Os pacotes de apps estão configurados para serem executados em uma arquitetura de processador específica. Ao selecionar uma arquitetura, você está especificando em quais dispositivos quer que seu app seja executado. Os aplicativos UWP (Plataforma Universal do Windows) podem ser configurados para serem executados nas seguintes arquiteturas:
- x86
- x64
- ARM
- ARM64

É **altamente** recomendado que você compile o pacote do aplicativo para todas as arquiteturas. Ao desmarcar uma arquitetura de dispositivo, você está limitando o número de dispositivos em que seu app pode ser executado e que, por sua vez, limitará a quantidade de pessoas que podem usar seu aplicativo!

## <a name="windows-10-devices-and-architectures"></a>Dispositivos e arquiteturas do Windows 10

> [!div class="mx-tableFixed"]
| Arquitetura UWP | Desktop (x86)      | Desktop (x86)      | Desktop (ARM)      | Móvel             | Realidade mista do Windows e HoloLens           | Xbox               | IoT Core (depende do dispositivo) | Hub de Superfície        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM               | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |
| ARM64              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :x:                |


Vamos falar sobre essas arquiteturas mais detalhadamente.

### <a name="x86"></a>x86
Escolher x86 é geralmente a configuração mais segura para um pacote de aplicativo, já que será executada em quase todos os dispositivos. Em alguns dispositivos, um pacote de aplicativo com a configuração x86 não será executado, como no Xbox ou em alguns dispositivos IoT Core. No entanto, para um PC, o pacote x86 é a opção mais segura e tem o maior alcance para implantação de dispositivo. Uma parte considerável de dispositivos Windows 10 continua a executar a versão x86 do Windows.

### <a name="x64"></a>x64
Essa configuração é usada com menos frequência do que a configuração x86. Observe que essa configuração é reservada para desktops que usam versões de 64 bits do Windows 10, [aplicativos UWP no Xbox](/windows/uwp/xbox-apps/system-resource-allocation) e o Windows 10 IoT Core no Intel Joule.

### <a name="arm-and-arm64"></a>ARM e ARM64
O Windows 10 na configuração do ARM inclui computadores desktop, dispositivos móveis e alguns dispositivos IoT Core (Rasperry Pi 2, Raspberry Pi 3 e DragonBoard). O Windows 10 em computadores desktop ARM são uma novidade para a família do Windows e, portanto, se você for um desenvolvedor de aplicativos UWP, deverá enviar pacotes ARM para a Loja para ter a melhor experiência nesses computadores.

>[!NOTE]
> Para criar seu aplicativo UWP para direcionar nativamente a plataforma ARM64, você deve ter o Visual Studio 2017 versão 15,9 ou posterior. Para obter mais informações, consulte [esta postagem do blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

Para obter mais informações, consulte [Windows 10 no ARM](/windows/uwp/porting/apps-on-arm). Confira esta //Build talk para ver uma demonstração do [Windows 10 em ARM](https://channel9.msdn.com/Events/Build/2017/P4171) e saiba mais sobre como ele funciona.

Para obter mais informações sobre tópicos específicos de IoT, consulte [implantando um aplicativo com o Visual Studio](/windows/iot-core/develop-your-app/AppDeployment).