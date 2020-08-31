---
title: Suporte para instaladores da área de trabalho que exigem a reinicialização do dispositivo
description: Descreve como dar suporte para instaladores da área de trabalho que exigem a reinicialização do dispositivo.
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, Ferramenta de Empacotamento MSIX, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b8c134d460764699cdcac082941df606df3c0bd3
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090774"
---
# <a name="support-for-desktop-installers-that-require-device-restart"></a>Suporte para instaladores da área de trabalho que exigem a reinicialização do dispositivo

A partir da versão 1.2019.701.0 da ferramenta de empacotamento MSIX, você pode disparar uma reinicialização durante a conversão para o instalador.

As reinicializações têm suporte por meio de nossa interface do usuário interativa e por meio da linha de comando. Além disso, por meio da linha de comando, você também pode especificar que deseja fazer logon automaticamente após a reinicialização para ter uma conversão sem intervenção. 

Você pode especificar quais códigos de saída indicam uma reinicialização na página Configurações > [código de saída do instalador](tool-best-practices.md#other-settings) . 

Dispare uma reinicialização por meio da interface do usuário durante o fluxo de trabalho de conversão ou por meio do menu Iniciar do computador de conversão. Após a reinicialização, você retornará ao mesmo local em que estava na ferramenta para continuar com a conversão.

Sempre queremos ouvir de você, portanto, se você tiver algum comentário, lembre-se de [enviar comentários](./insider-program.md#share-your-feedback) conosco.