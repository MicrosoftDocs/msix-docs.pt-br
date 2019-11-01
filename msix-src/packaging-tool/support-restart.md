---
title: Suporte para instaladores da área de trabalho que exigem a reinicialização do dispositivo
description: Descreve como dar suporte para instaladores da área de trabalho que exigem a reinicialização do dispositivo.
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, Ferramenta de Empacotamento MSIX, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2be11523b1af099b168a9d7744dc3f11bd7407df
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328896"
---
# <a name="support-for-desktop-installers-that-require-device-restart"></a>Suporte para instaladores da área de trabalho que exigem a reinicialização do dispositivo

 > [!NOTE] 
 > A reinicialização do dispositivo é um novo recurso que atualmente só está disponível no build do Insider Preview.
 > Você pode ingressar no Programa Insider Preview da Ferramenta de Empacotamento MSIX para obter acesso a esse recurso.

Agora, damos suporte à capacidade de reinicialização durante o processo de conversão usando a Ferramenta de Empacotamento MSIX. 

Há suporte para reinicializações por meio de nossa interface do usuário interativa e de nossa interface de linha de comando. Além disso, por meio da linha de comando, você também pode especificar que deseja fazer logon automático após a reinicialização, a fim de ter uma conversão direta de mãos livres. 

Especifique quais códigos de saída indicam uma reinicialização na **página Configurações (Códigos de saída do instalador)** . 

Dispare uma reinicialização por meio da interface do usuário durante o fluxo de trabalho de conversão ou por meio do menu Iniciar do computador de conversão. Após a reinicialização, você retornará ao mesmo local em que estava na ferramenta para continuar com a conversão.

Tenha em mente que esse recurso ainda está em desenvolvimento e pode haver alguns bugs. Se você encontrar algum, não se esqueça de [enviar comentários](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program#share-your-feedback) para que possamos corrigi-lo.

