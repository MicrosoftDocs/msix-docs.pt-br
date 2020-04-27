---
Description: Este guia contém instruções sobre como criar e implantar um aplicativo MSIX com o Microsoft Endpoint Configuration Manager.
title: Microsoft Endpoint Configuration Manager
ms.date: 1/31/2020
ms.topic: article
keywords: windows 10, uwp, mecm, configmgr, app, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: eb136e71cec242efebe9129d2c6c8bee5fa62a35
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "77074172"
---
# <a name="microsoft-endpoint-configuration-manager"></a>Microsoft Endpoint Configuration Manager
O [Microsoft Endpoint Configuration Manager](https://docs.microsoft.com/configmgr/) pode ser usado para criar um `Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)` que pode ser distribuído para dispositivos cliente. A implantação de um aplicativo MSIX tem os seguintes pré-requisitos:
1) Acesso ao pacote MSIX a ser implantado.
2) uma coleção de dispositivos como destino da instalação.

## <a name="deploying-msix-application"></a>Implantar o aplicativo MSIX
Ao implantar um pacote de aplicativo do Windows pelo Console do Microsoft Endpoint Configuration Manager a ser distribuído, você primeiro vai precisar de um caminho UNC para o pacote do aplicativo MSIX. Os detalhes do aplicativo sendo criado serão recuperados pelos metadados contidos no pacote do aplicativo MSIX e serão adicionados automaticamente às propriedades do pacote do aplicativo do Windows.

|||
|-----|------|
| 1. Capturar um pacote do MSIX do Instalador ou recuperar um aplicativo MSIX existente | [Criar um pacote do MSIX de um instalador de área de trabalho](../packaging-tool/create-app-package-msi-vm.md)  |
| 2. Iniciar o Console do Microsoft Endpoint Configuration Manager | [Console do Microsoft Endpoint Configuration Manager](https://devicemanagement.microsoft.com) |
| 3. Criar um aplicativo de linha de negócios | Veja [Criar um pacote de aplicativo do Windows](https://docs.microsoft.com/configmgr/apps/get-started/creating-windows-applications) |
| 4. Criar um usuário ou uma coleção de dispositivos | [Criação de coleções](https://docs.microsoft.com/configmgr/core/clients/manage/collections/create-collections) |
| 5. Implantar um aplicativo de linha de negócios | [Implantação de aplicativos com o ConfigMgr](https://docs.microsoft.com/configmgr/apps/deploy-use/deploy-applications) |
