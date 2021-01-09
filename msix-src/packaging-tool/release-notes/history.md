---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 2a63cd0927216d71b5a8006b646a001817249c5c
ms.sourcegitcommit: 3a9aae783331833bbf244091544c48848768137e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98041107"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas sobre a versão da Ferramenta de Empacotamento MSIX

## <a name="version-1202012190"></a>1.2020.1219.0 da versão
- Foi removido o suporte à versão 1 da assinatura do Device Guard. Consulte a documentação sobre como usar a [versão 2](../../package/signing-package-device-guard-signing.md). Se tiver alguma dúvida, entre em contato com a equipe de suporte da assinatura do Device Guard DGSSMigration@microsoft.com

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

