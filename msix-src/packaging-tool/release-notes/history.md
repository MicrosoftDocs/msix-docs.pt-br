---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 5371305d2ae1b1b76e76236913393aab7fd4b241
ms.sourcegitcommit: 77e8b10706d4ad2fa97b2c71d82947528555398b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691692"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas sobre a versão da Ferramenta de Empacotamento MSIX

## <a name="version-1202011300"></a>1.2020.1130.0 da versão
- Correção de um problema em que clicar na mesma linha em arquivos de pacote não funcionou para realçar o item
- Aprimoramentos de UX para a página Selecionar instalador
  - Permitir que instaladores no caminho sejam especificados witout fornecendo o caminho completo
  - Mostrar a cadeia de caracteres de linha de comando usada para executar o instalador selecionado
  - Estenda a caixa de texto argumentos do instalador para cadeias de caracteres de argumento longo para que sejam mais fáceis de revisar e editar
- Somente adicione um logotipo padrão para a loja se um não for extraído do aplicativo

## <a name="version-1202010060---public-release"></a>Versão 1.2020.1006.0 – versão pública
- Suporte adicionado para a [versão 2 da assinatura do Device Guard](../../package/signing-package-device-guard-signing.md). Se tiver alguma dúvida, entre em contato com a equipe de suporte da assinatura do Device Guard DGSSMigration@microsoft.com
- Adicionada uma correção para MaxVersionTested para manter o manifesto atualizado
- Correção de bug para preservar ícones em atalhos para conversões do App-V
- Adicionado um relatório de serviços ao editor de pacote
- Correções gerais de bugs

