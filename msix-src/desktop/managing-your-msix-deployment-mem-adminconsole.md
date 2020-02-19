---
Description: Este guia contém instruções sobre como criar e implantar um aplicativo MSIX com o Console de Administração do Gerenciador de Ponto de Extremidade da Microsoft
title: Console de administração do Gerenciador de Ponto de Extremidade da Microsoft
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, console de administração do mem, aplicativo
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f0ed835c3eb94dda358fbf88fe587bcb44277af1
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073912"
---
# <a name="microsoft-endpoint-manager-admin-console"></a>Console de administração do Gerenciador de Ponto de Extremidade da Microsoft
O [Console de Administração do Gerenciador de Ponto de Extremidade da Microsoft](https://devicemanagement.microsoft.com) pode ser usado para implantar um pacote MSIX em dispositivos cliente. Ao implantar um aplicativo MSIX pelo console de administração do Microsoft Endpoint Configuration Manager, será necessário ter um aplicativo MSIX a ser implantado, bem como um Grupo do Azure Active Directory para ser o destino da instalação.

## <a name="deploying-msix-application"></a>Implantar o aplicativo MSIX
Ao implantar um aplicativo pelo Microsoft Endpoint Configuration Manager para ser distribuído, primeiro será necessário um caminho local ou de rede para o pacote MSIX. Os detalhes do aplicativo sendo criado pelo Microsoft Endpoint Configuration Manager recuperarão os metadados contidos no pacote do aplicativo MSIX e carregarão automaticamente as informações recuperadas nas propriedades do Aplicativo.

|||
|-----|------|
| 1. Capturar um pacote do MSIX do Instalador ou recuperar um aplicativo MSIX existente | [Criar um pacote do MSIX de um instalador de área de trabalho](../packaging-tool/create-app-package-msi-vm.md)  |
| 2. Iniciar o Console de Administração do Microsoft Endpoint Configuration Manager | [Console de Administração do Microsoft Endpoint Configuration Manager](https://devicemanagement.microsoft.com) |
| 3. Criar e implantar um aplicativo de linha de negócios | [Criar e implantar um aplicativo de linha de negócios](https://docs.microsoft.com/intune/apps/lob-apps-windows) |
