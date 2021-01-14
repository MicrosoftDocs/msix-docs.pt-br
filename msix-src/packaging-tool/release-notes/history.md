---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 43b605706eafb0ac028c0113fc79dfedef2d7bbe
ms.sourcegitcommit: 789ba344260b0afd46206f94954de3120aefb91e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177655"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas sobre a versão da Ferramenta de Empacotamento MSIX

## <a name="version-1202012190---public-version"></a>Versão 1.2020.1219.0 – versão pública
- Foi removido o suporte à versão 1 da assinatura do Device Guard. Consulte a documentação sobre como usar a [versão 2](../../package/signing-package-device-guard-signing.md). Se tiver alguma dúvida, entre em contato com a equipe de suporte da assinatura do Device Guard DGSSMigration@microsoft.com
- Correção de um problema em que clicar na mesma linha em arquivos de pacote não funcionou para realçar o item
- Aprimoramentos de UX para a página Selecionar instalador
  - Permitir que instaladores no caminho sejam especificados witout fornecendo o caminho completo
  - Mostrar a cadeia de caracteres de linha de comando usada para executar o instalador selecionado
  - Estenda a caixa de texto argumentos do instalador para cadeias de caracteres de argumento longo para que sejam mais fáceis de revisar e editar
- Somente adicione um logotipo padrão para a loja se um não for extraído do aplicativo
- Correções gerais de bugs
