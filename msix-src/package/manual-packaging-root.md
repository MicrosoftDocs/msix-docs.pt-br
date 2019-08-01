---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Empacotamento manual de app
description: Esta seção contém ou links para artigos sobre o empacotamento manual de aplicativos do Windows.
ms.date: 07/30/2019
ms.topic: article
keywords: windows 10, uwp, empacotamento
ms.localizationpriority: medium
ms.openlocfilehash: b1e367050f0c1b84aafb0ead037120bd24dea448
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689985"
---
# <a name="manual-app-packaging"></a>Empacotamento manual de app

Se você quiser criar e assinar um pacote de aplicativo do Windows, mas não usou o Visual Studio para desenvolver seu aplicativo, você precisará usar as ferramentas de empacotamento manual do aplicativo.

> [!IMPORTANT] 
> Se você usou o Visual Studio para desenvolver seu aplicativo do Windows, é recomendável usar o assistente do Visual Studio para criar e assinar o pacote do aplicativo. Para obter mais informações, consulte [empacotar um aplicativo UWP com o Visual Studio](packaging-uwp-apps.md) e [empacotar um aplicativo de desktop do código-fonte usando o Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

## <a name="purpose"></a>Finalidade

Esta seção contém ou links para artigos sobre o empacotamento manual de aplicativos do Windows.

| Tópico | Descrição |
|-------|-------------|
| [Criar um pacote de aplicativo com a ferramenta MakeAppx. exe](create-app-package-with-makeappx-tool.md) | A MakeAppx.exe cria, criptografa, descriptografa e extrai arquivos de pacotes e lotes de aplicativos. |
| [Empacotar um aplicativo de área de trabalho manualmente](../desktop/desktop-to-uwp-manual-conversion.md) | Saiba como usar o **MakeApp. exe** para empacotar um aplicativo de área de trabalho. |
| [Criar um certificado para assinatura de pacote](create-certificate-package-signing.md) | Criar e exportar um certificado para a assinatura de pacote de apps com as ferramentas do PowerShell. |
| [Assinar um pacote de aplicativo usando SignTool](sign-app-package-using-signtool.md) | Use SignTool para assinar um pacote de aplicativos com um certificado manualmente. |

### <a name="advanced-topics"></a>Tópicos avançados

Esta seção contém tópicos mais avançados para inserir componentes em um aplicativo grande e/ou complexo para um empacotamento e a instalação mais eficientes. 

> [!IMPORTANT]
> Se você pretende enviar seu aplicativo para a Store, você precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) e receber aprovação para usar qualquer um dos recursos avançados nesta seção.


| Tópico | Descrição |
|-------|-------------|
| [Introdução aos pacotes de ativos](asset-packages.md) | Pacotes de ativo são um tipo de pacote que atuam como um local centralizado para arquivos comuns de um aplicativo, eliminando efetivamente a necessidade de arquivos duplicados através de seus pacotes de arquitetura. |
| [Desenvolvendo com pacotes de ativos e dobramento de pacote](package-folding.md) | Saiba como organizar de forma eficiente seu aplicativo com pacotes de ativo e dobramento de pacote. |
| [Pacotes de aplicativos de pacote simples](flat-bundles.md) | Descreve como criar um pacote simples para os arquivos de pacote do aplicativo. |
| [Criação de pacote com o layout de empacotamento](packaging-layout.md) | O layout de empacotamento é um documento único que descreve a estrutura de empacotamento do aplicativo. Ele especifica os pacotes de um aplicativo (principal e opcional), os pacotes no lote e os arquivos nos pacotes. |
