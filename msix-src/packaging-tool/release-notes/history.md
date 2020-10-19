---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: c694256f43c91278ad7ffaa7917c3b43983d486f
ms.sourcegitcommit: 79a5d5b901b123ec2590d3b9fb305f281a9bcc1f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92169239"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas sobre a versão da Ferramenta de Empacotamento MSIX

## <a name="version-1202010060"></a>1.2020.1006.0 da versão
- Suporte adicionado para a [versão 2 da assinatura do Device Guard](../../package/signing-package-device-guard-signing.md). Se tiver alguma dúvida, entre em contato com a equipe de suporte da assinatura do Device Guard DGSSMigration@microsoft.com
- Adicionada uma correção para MaxVersionTested para manter o manifesto atualizado
- Correção de bug para preservar ícones em atalhos para conversões do App-V

## <a name="version-120208240"></a>1.2020.824.0 da versão
- Adicionado um relatório de serviços ao editor de pacote
- Correções gerais de bugs

## <a name="version-120207090---public-release"></a>Versão 1.2020.709.0 – versão pública
- Capacidade de adicionar vários arquivos ao editor de pacotes
- Capacidade de importar vários arquivos. reg para o editor de pacote
- Suporte aprimorado para converter qualquer tipo de instalador
- Capacidade de adicionar itens do editor de pacote à lista de exclusões
- Usar Ctrl para opções de seleção múltipla no editor de pacote
- Adicionou um prompt ao substituir os arquivos ao mover ou adicionar bugs fixos:
- Desabilite a seleção da máquina VM local quando o Hyper-V não estiver presente
- A caixa de seleção impor requisitos de repositório está marcada, impedir a criação de ícones não aceitos pela loja
- Corrigido um problema com os valores do registro do App-V durante a conversão
- Adicionada uma sessão de tempo limite mais longa para conversões de linha de comando remotas
- Melhores seleções do so MSIX Core para reduzir conflitos e confusão

