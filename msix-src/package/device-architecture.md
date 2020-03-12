---
title: Arquiteturas de pacote do aplicativo
description: Saiba mais sobre quais arquiteturas de processador você deve usar ao criar seu pacote do aplicativo UWP.
ms.date: 07/13/2017
ms.topic: article
keywords: Windows 10, UWP, empacotamento, arquitetura, configuração de pacote
ms.localizationpriority: medium
ms.openlocfilehash: f64c4ae114d334cb614c417ca45616cc50cd0654
ms.sourcegitcommit: fa41875f6c2b79db3d7dde29b10c0f24765532bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79097911"
---
# <a name="app-package-architectures"></a>Arquiteturas de pacote do aplicativo

Os pacotes de aplicativos são configurados para serem executados em uma arquitetura de processador específica. Ao selecionar uma arquitetura, você está especificando quais dispositivos você deseja que seu aplicativo execute. Os aplicativos Plataforma Universal do Windows (UWP) podem ser configurados para serem executados nas seguintes arquiteturas:
- x86
- x64
- {1&gt;ARM&lt;1}
- ARM64

É **altamente** recomendável que você crie seu pacote de aplicativo para direcionar todas as arquiteturas. Ao desmarcar a arquitetura de um dispositivo, você está limitando o número de dispositivos em que seu aplicativo pode ser executado, o que, por sua vez, limitará a quantidade de pessoas que podem usar seu aplicativo!

## <a name="windows-10-devices-and-architectures"></a>Dispositivos e arquiteturas do Windows 10

> [!div class="mx-tableFixed"]
| Arquitetura UWP | Área de trabalho (x86)      | Área de trabalho (x64)      | Área de trabalho (ARM)      | Mobilidade             | Realidade mista do Windows e HoloLens           | Xbox               | IoT Core (dependente do dispositivo) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | w.x.y.                | :heavy_check_mark: | w.x.y.                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | w.x.y.                | :heavy_check_mark: | w.x.y.                | w.x.y.                | w.x.y.                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| {1&gt;ARM&lt;1}               | w.x.y.                | w.x.y.                | :heavy_check_mark: | :heavy_check_mark: | w.x.y.                | w.x.y.                | :heavy_check_mark:          | w.x.y.                |
| ARM64              | w.x.y.                | w.x.y.                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | w.x.y.                | :heavy_check_mark:          | w.x.y.                |


Vamos falar sobre essas arquiteturas mais detalhadamente.

### <a name="x86"></a>x86
Escolher x86 é geralmente a configuração mais segura para um pacote de aplicativo, pois ele será executado em quase todos os dispositivos. Em alguns dispositivos, um pacote de aplicativo com a configuração x86 não será executado, como o Xbox ou alguns dispositivos IoT Core. No entanto, para um PC, um pacote x86 é a opção mais segura e tem o maior alcance para a implantação de dispositivos. Uma parte substancial dos dispositivos Windows 10 continua a executar a versão x86 do Windows.

### <a name="x64"></a>x64
Essa configuração é usada com menos frequência do que a configuração x86. Deve-se observar que esse configuração é reservado para desktops usando versões de 64 bits do Windows 10, [aplicativos UWP no Xbox](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation)e Windows 10 IOT Core no Intel Joule.

### <a name="arm-and-arm64"></a>ARM e ARM64
A configuração do Windows 10 no ARM inclui computadores desktop, dispositivos móveis e alguns dispositivos IoT Core (Rasperry pi 2, Raspberry Pi 3 e DragonBoard). O Windows 10 em computadores desktop ARM é uma nova adição à família Windows, portanto, se você for um desenvolvedor de aplicativos UWP, deverá enviar pacotes ARM para a loja para obter a melhor experiência nesses PCs.

>[!NOTE]
> Para criar seu aplicativo UWP para direcionar nativamente a plataforma ARM64, você deve ter o Visual Studio 2017 versão 15,9 ou posterior. Para obter mais informações, consulte [esta postagem no blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

Para obter mais informações, consulte [Windows 10 no ARM](https://docs.microsoft.com/windows/uwp/porting/apps-on-arm.md). Confira este//Build Talk para ver uma demonstração do [Windows 10 no ARM](https://channel9.msdn.com/Events/Build/2017/P4171) e saiba mais sobre como ele funciona.

Para obter mais informações sobre tópicos específicos de IoT, consulte [implantando um aplicativo com o Visual Studio](https://developer.microsoft.com/windows/iot/Docs/AppDeployment).
