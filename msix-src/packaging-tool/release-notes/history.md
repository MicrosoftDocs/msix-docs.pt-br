---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Histórico completo das notas de versão da ferramenta de empacotamento MSIX
author: c-don
ms.author: cdon
ms.date: 03/05/2019
ms.topic: article
keywords: Windows 10, uwp, msix, ferramenta de empacotamento msix, programa insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 6eb6f4cedc8b25046450080c0a6a2e07b0a4fb12
ms.sourcegitcommit: d505622e4f89daa0a9e7c7df0bd2228fd8ab792d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67694976"
---
# <a name="msix-packaging-tool-release-notes"></a>Notas de versão de ferramenta de empacotamento MSIX

## <a name="version-120197010---public-release"></a>Versão 1.2019.701.0 - lançamento público

- Suporte para instaladores da área de trabalho que exigem reinicialização - [Saiba mais](../support-restart.md)
    - Opção de logon automático para reinicialização 
- Novas opções nas configurações do aplicativo
    - Especifique um certificado padrão para assinar os pacotes com 
    - Especifique os códigos de saída para instaladores que exigem reinicialização
    - Código de saída para a lista de reinicializações nas configurações inclui os valores padrão comuns
- Capacidade de adicionar recursos novos, desconhecidos e personalizados por meio do editor de pacote
- Define automaticamente MinVersion para quando os requisitos de controle de versão do Store estiverem desativados nas configurações do 1709
- Novas pastas podem ser adicionadas em ativos no editor de pacote
- Restaurar as configurações padrão e itens de exclusão agora também limpa os códigos de senha e sair do certificado de assinatura
- Corrigido um problema em que as primeiras tarefas de inicialização não foram obtendo excluídas corretamente
- Irá ignorar os atalhos para arquivos excluídos durante a criação do pacote
- O padrão é Assinando um pacote, se um certificado de assinatura padrão é especificado nas configurações de
- Respeitar os códigos de saída do instalador do PowerShell
- Informa ao usuário quando eles precisarem de uma reinicialização do driver
