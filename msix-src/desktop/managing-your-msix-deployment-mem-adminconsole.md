---
description: Este guia contém instruções sobre como criar e implantar um aplicativo MSIX com o Console de Administração do Microsoft Endpoint Manager
title: Console de administração do Microsoft Endpoint Manager
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, console de administração do mem, aplicativo
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6073f787630751df88734f293b65e63a90bebc02
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090514"
---
# <a name="microsoft-endpoint-manager-admin-console"></a>Console de administração do Microsoft Endpoint Manager
O [Console de Administração do Microsoft Endpoint Manager](https://devicemanagement.microsoft.com) pode ser usado para implantar um pacote MSIX em dispositivos cliente. Ao implantar um aplicativo MSIX pelo console de administração do Microsoft Endpoint Configuration Manager, será necessário ter um aplicativo MSIX a ser implantado, bem como um Grupo do Azure Active Directory para ser o destino da instalação.

## <a name="deploying-msix-application"></a>Implantar o aplicativo MSIX
Ao implantar um aplicativo pelo Console de Admin do Microsoft Endpoint Manager para ser distribuído, primeiro será necessário um caminho local ou de rede para o pacote MSIX. Os detalhes do aplicativo sendo criado pelo Console de Admin do Microsoft Endpoint Manager recuperarão os metadados contidos no pacote do aplicativo MSIX e carregarão automaticamente as informações recuperadas nas propriedades do Aplicativo.

|||
|-----|------|
| 1. Capturar um pacote do MSIX do Instalador ou recuperar um aplicativo MSIX existente | [Criar um pacote do MSIX de um instalador de área de trabalho](../packaging-tool/create-app-package.md)  |
| 2. Iniciar o Console de Admin do Microsoft Endpoint Manager | [Console de Admin do Microsoft Endpoint Manager](https://devicemanagement.microsoft.com) |
| 3. Criar e implantar um aplicativo de linha de negócios | [Criar e implantar um aplicativo de linha de negócios](/intune/apps/lob-apps-windows) |
