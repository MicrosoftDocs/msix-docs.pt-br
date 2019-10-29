---
Description: Este artigo descreve os requisitos de assinatura para aplicativos do Windows 10.
title: Assinar um pacote do aplicativo do Windows 10
ms.date: 07/03/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: ddd7a8769bd2d09122153cc42aecd4d686e25d40
ms.sourcegitcommit: f47c140e2eb410c2748be7912955f43e7adaa8f9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72776459"
---
# <a name="sign-a-windows-10-app-package"></a>Assinar um pacote do aplicativo do Windows 10

A assinatura de pacote do aplicativo é uma etapa necessária no processo de criação de um pacote do aplicativo do Windows 10 que pode ser implantado. O Windows 10 exige que todos os aplicativos sejam assinados com um certificado de assinatura de código válido.

Para instalar um aplicativo do Windows 10 com êxito, além de o pacote ser assinado, ele também precisa estar identificado como confiável no dispositivo. Isso significa que o certificado precisa ser encadeado para uma das raízes confiáveis no dispositivo. Por padrão, o Windows 10 confia em certificados da maioria das autoridades de certificação que fornecem certificados de assinatura de código.

|Tópico| Descrição |
|:---|:---|
|[Pré-requisitos para assinatura](sign-app-package-using-signtool.md#prerequisites)| Esta seção aborda os pré-requisitos necessários para assinar o pacote do aplicativo do Windows 10. | 
|[Como usar a SignTool](sign-app-package-using-signtool.md#using-signtool)| Esta seção aborda como usar a SignTool do SDK do Windows 10 para assinar o pacote do aplicativo.|

## <a name="timestamping"></a>Carimbo de data/hora

Além da assinatura do pacote do aplicativo com um certificado, outra característica importante que determina a validade do certificado de assinatura de código é o **carimbo de data/hora**. O carimbo de data/hora preserva a assinatura, permitindo que o pacote do aplicativo seja aceito pela plataforma de implantação do aplicativo, mesmo após a expiração do certificado. No momento da inspeção do pacote, o carimbo de data/hora permite que a assinatura do pacote seja validada em relação à hora em que ele foi assinado. Isso permite que os pacotes sejam aceitos, mesmo depois que o certificado não seja mais válido. Os pacotes que não têm o carimbo de data/hora serão avaliados em relação à hora atual e, se o certificado não for mais válido, o Windows não aceitará o pacote. 

Estes são os diferentes cenários referentes ao carimbo de data/hora com e sem a assinatura do aplicativo:

| |O aplicativo é assinado sem o carimbo de data/hora | O aplicativo é assinado com o carimbo de data/hora |
|---|---------------------------------- | ------------------------------- |
| O certificado é válido |O aplicativo será instalado | O aplicativo será instalado |
| O certificado é inválido (expirado) | O aplicativo não será instalado | O aplicativo será instalado, pois a autenticidade do certificado foi verificada durante a assinatura pela autoridade de carimbo de data/hora |

 > [!NOTE]
 > Se o aplicativo for instalado com êxito em um dispositivo, ele continuará sendo executado mesmo após a expiração do certificado, independentemente de ele ter ou não o carimbo de data/hora. 

## <a name="device-mode"></a>Modo do dispositivo

O Windows 10 permite que os usuários selecionem o modo no qual seus dispositivos serão executados no aplicativo de Configurações. Os modos são Aplicativos da Microsoft Store, Aplicativos de sideload e Modo de desenvolvedor. 

**Aplicativos da Microsoft Store** é a opção mais segura, pois só permite a instalação de aplicativos da Microsoft Store. Os aplicativos da Microsoft Store passam por um processo de certificação para garantir que sejam seguros para uso. 

**Aplicativos de sideload** e **Modo de desenvolvedor** são opções mais permissivas de aplicativos que são assinados por outros certificados, desde que esses certificados sejam confiáveis e encadeados para uma das raízes confiáveis no dispositivo. Só selecione o Modo de desenvolvedor se você for um desenvolvedor e estiver compilando ou depurando aplicativos do Windows 10. Encontre mais informações sobre o Modo de desenvolvedor e o que ele oferece [aqui](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development). 
