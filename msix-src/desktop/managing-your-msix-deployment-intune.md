---
description: Este guia oferece instruções sobre como criar e implantar um aplicativo MSIX com o Microsoft Intune.
title: Microsoft Intune
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, console de admin mem, app, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: decb8a73bf7403bfe7e82ebfdbff468b1b884f2c
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090524"
---
# <a name="microsoft-intune"></a>Microsoft Intune
O [Console do Microsoft Intune](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) pode ser usado para criar um Aplicativo de Linha de Negócios que pode ser distribuído para dispositivos cliente. Ao implantar um aplicativo MSIX pelo Microsoft Intune, será necessário ter um aplicativo MSIX a ser implantado, bem como um Grupo do Azure Active Directory como destino da instalação.

## <a name="deploying-an-msix-app"></a>Implantação de um aplicativo MSIX
Ao implantar um aplicativo cliente pelo Microsoft Intune para ser distribuído para dispositivos cliente, primeiro será necessário ter um caminho local ou de rede para o aplicativo MSIX. Os detalhes do aplicativo sendo criados pelo Microsoft Intune recuperarão os metadados contidos no pacote do aplicativo MSIX e carregarão automaticamente as informações recuperadas nas propriedades do aplicativo.

|||
|-----|------|
| 1. Capturar um pacote do MSIX do Instalador ou recuperar um aplicativo MSIX existente | [Criar um pacote do MSIX de um instalador de área de trabalho](../packaging-tool/create-app-package.md) |
| 2. Iniciar o Console do Intune | [Console do Microsoft Intune](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) |
| 3. Criar e implantar um aplicativo de linha de negócios | [Criar e implantar um aplicativo de linha de negócios](/intune/apps/lob-apps-windows) |
