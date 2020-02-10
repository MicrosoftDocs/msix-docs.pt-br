---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Pacote da linha de comando
description: Esta seção contém ou links para artigos sobre o empacotamento manual de aplicativos do Windows.
ms.date: 01/30/2020
ms.topic: article
keywords: windows 10, uwp, empacotamento
ms.localizationpriority: medium
ms.openlocfilehash: 9bbc940360ed7c3c0a428ba4aa3f981274c0a5d7
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072677"
---
# <a name="package-from-the-command-line"></a>Pacote da linha de comando

Se você não desenvolver seu aplicativo no Visual Studio, poderá usar as ferramentas de linha de comando MSIX para empacotar e assinar seus aplicativos.


## <a name="purpose"></a>Finalidade

Esta seção contém links para artigos sobre como empacotar manualmente seu aplicativo como um MSIX usando ferramentas de linha de comando.

| Tópico | Descrição |
|-------|-------------|
| [Gerando componentes de pacote](../desktop/desktop-to-uwp-manual-conversion.md) | Criar um manifesto de pacote e adicionar ativos não folheados baseados em destino (opcional) |
| [Criar um pacote ou grupo MSIX com a ferramenta MakeAppx. exe](create-app-package-with-makeappx-tool.md) | A MakeAppx.exe cria, criptografa, descriptografa e extrai arquivos de pacotes e lotes de aplicativos. |
| [Criar um certificado para assinatura de pacote](create-certificate-package-signing.md) | Criar e exportar um certificado para a assinatura de pacote de apps com as ferramentas do PowerShell. |
| [Assinar um pacote de aplicativo usando SignTool](sign-app-package-using-signtool.md) | Use SignTool para assinar um pacote de aplicativos com um certificado manualmente. |

### <a name="advanced-topics"></a>Tópicos Avançados

Esta seção contém tópicos mais avançados para inserir componentes em um aplicativo grande e/ou complexo para um empacotamento e a instalação mais eficientes. 

> [!IMPORTANT]
> Se você pretende enviar seu aplicativo para a Store, você precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) e receber aprovação para usar qualquer um dos recursos avançados nesta seção.


| Tópico | Descrição |
|-------|-------------|
| [Criação de pacote com o layout de empacotamento](packaging-layout.md) | O layout de empacotamento é um documento único que descreve a estrutura de empacotamento do aplicativo. Ele especifica os pacotes de um aplicativo (principal e opcional), os pacotes no lote e os arquivos nos pacotes. |
| [Introdução aos pacotes de ativos](asset-packages.md) | Pacotes de ativo são um tipo de pacote que atuam como um local centralizado para arquivos comuns de um aplicativo, eliminando efetivamente a necessidade de arquivos duplicados através de seus pacotes de arquitetura. |
| [Desenvolvendo com pacotes de ativos e dobramento de pacote](package-folding.md) | Saiba como organizar de forma eficiente seu aplicativo com pacotes de ativo e dobramento de pacote. |
| [Pacotes de aplicativos de pacote simples](flat-bundles.md) | Descreve como criar um pacote simples para os arquivos de pacote do aplicativo. |

