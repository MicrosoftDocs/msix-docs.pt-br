---
description: Este guia contém instruções sobre como criar e implantar um aplicativo MSIX com o Microsoft Endpoint Configuration Manager.
title: Microsoft Endpoint Configuration Manager
ms.date: 1/31/2020
ms.topic: article
keywords: windows 10, uwp, mecm, configmgr, app, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 51b681af964ad55822bb27768a17372ed3aebadf
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090604"
---
# <a name="microsoft-endpoint-configuration-manager"></a>Microsoft Endpoint Configuration Manager
O [Microsoft Endpoint Configuration Manager](/configmgr/) pode ser usado para criar um `Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)` que pode ser distribuído para dispositivos cliente. A implantação de um aplicativo MSIX tem os seguintes pré-requisitos:
1) Acesso ao pacote MSIX a ser implantado.
2) uma coleção de dispositivos como destino da instalação.

## <a name="deploying-msix-application"></a>Implantar o aplicativo MSIX
Ao implantar um pacote de aplicativo do Windows pelo Console do Microsoft Endpoint Configuration Manager a ser distribuído, você primeiro vai precisar de um caminho UNC para o pacote do aplicativo MSIX. Os detalhes do aplicativo sendo criado serão recuperados pelos metadados contidos no pacote do aplicativo MSIX e serão adicionados automaticamente às propriedades do pacote do aplicativo do Windows.

|||
|-----|------|
| 1. Capturar um pacote do MSIX do Instalador ou recuperar um aplicativo MSIX existente | [Criar um pacote do MSIX de um instalador de área de trabalho](../packaging-tool/create-app-package.md)  |
| 2. Iniciar o Console do Microsoft Endpoint Configuration Manager | [Console do Microsoft Endpoint Configuration Manager](https://devicemanagement.microsoft.com) |
| 3. Criar um aplicativo de linha de negócios | Veja [Criar um pacote de aplicativo do Windows](/configmgr/apps/get-started/creating-windows-applications) |
| 4. Criar um usuário ou uma coleção de dispositivos | [Criação de coleções](/configmgr/core/clients/manage/collections/create-collections) |
| 5. Implantar um aplicativo de linha de negócios | [Implantação de aplicativos com o ConfigMgr](/configmgr/apps/deploy-use/deploy-applications) |
