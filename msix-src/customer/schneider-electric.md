---
title: Schneider elétrico
description: O Schneider Electric descreve sua experiência com o MSIX
author: dianmsft
ms.date: 06/25/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8c095897ea6a8b58c32e265a26066305ec5e4a05
ms.sourcegitcommit: fe85d57b3c9478de5fd5347010d2dee1e73f2268
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85549077"
---
# <a name="schneider-electric"></a>Schneider elétrico

![Schneider elétrico](../images/Logo_SE_Green_RGB-Screen.png)

O Schneider Electric desenvolve o software GIS para dar suporte a utilitários pequenos e grandes (elétricos, gás, água, fibra, insistir) nos EUA e internacionalmente.

Ao trabalhar com a Microsoft, queríamos trazer nossos aplicativos do WPF para a tecnologia de instalação mais recente, MSIX. Temos um caso de uso específico baseado em cliente; Precisamos lançar e implantar com frequência como uma empresa de software de opinião de desenvolvimento, mas cada empresa de utilitário deseja gerenciar as alterações e determinar individualmente qual versão do aplicativo será liberada para seus usuários e quando. Estamos trabalhando junto com a Microsoft para atender a esse requisito de cliente. Depois de concluirmos nossa atualização para MSIX, poderemos remover algumas dependências de terceiros e ser um passo mais próximo de atualizar nossos aplicativos para o .NET Core 3,0.

Aproveitando o MSIX, estamos simplificando nossa pilha de desenvolvimento. Usaremos as tecnologias da Microsoft do código (C#/WPF) para compilar (Azure dev ops) para o instalador (MSIX). Um dos principais benefícios do MSIX é que os desenvolvedores podem facilmente desenvolver, depurar e testar o instalador diretamente do Visual Studio. Isso simplifica muito a instalação de uma solução de problemas específica e nos permitiu trabalhar em conjunto com a Microsoft.

*"O suporte da Microsoft à medida que mudamos para MSIX é inestimável. Temos um caso de uso exclusivo, e a Microsoft nos ajudou a navegar pelos obstáculos encontrados durante a atualização de nossos aplicativos. Os desenvolvedores estão empolgados em migrar nossos aplicativos para MSIX. "* – Darlene Rouleau da Schneider Electric
