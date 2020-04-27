---
Description: Este guia contém instruções sobre como criar e implantar um aplicativo MSIX com o Console de Administração do Microsoft Endpoint Manager
title: Console de administração do Microsoft Endpoint Manager
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, console de administração do mem, aplicativo
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f0ed835c3eb94dda358fbf88fe587bcb44277af1
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "77073912"
---
# <a name="microsoft-endpoint-manager-admin-console"></a>Console de administração do Microsoft Endpoint Manager
O [Console de Administração do Microsoft Endpoint Manager](https://devicemanagement.microsoft.com) pode ser usado para implantar um pacote MSIX em dispositivos cliente. Ao implantar um aplicativo MSIX pelo console de administração do Microsoft Endpoint Configuration Manager, será necessário ter um aplicativo MSIX a ser implantado, bem como um Grupo do Azure Active Directory para ser o destino da instalação.

## <a name="deploying-msix-application"></a>Implantar o aplicativo MSIX
Ao implantar um aplicativo pelo Console de Admin do Microsoft Endpoint Manager para ser distribuído, primeiro será necessário um caminho local ou de rede para o pacote MSIX. Os detalhes do aplicativo sendo criado pelo Console de Admin do Microsoft Endpoint Manager recuperarão os metadados contidos no pacote do aplicativo MSIX e carregarão automaticamente as informações recuperadas nas propriedades do Aplicativo.

|||
|-----|------|
| 1. Capturar um pacote do MSIX do Instalador ou recuperar um aplicativo MSIX existente | [Criar um pacote do MSIX de um instalador de área de trabalho](../packaging-tool/create-app-package-msi-vm.md)  |
| 2. Iniciar o Console de Admin do Microsoft Endpoint Manager | [Console de Admin do Microsoft Endpoint Manager](https://devicemanagement.microsoft.com) |
| 3. Criar e implantar um aplicativo de linha de negócios | [Criar e implantar um aplicativo de linha de negócios](https://docs.microsoft.com/intune/apps/lob-apps-windows) |
