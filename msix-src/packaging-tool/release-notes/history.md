---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Histórico completo das notas de versão da ferramenta de empacotamento MSIX
ms.date: 03/05/2019
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 03a5b366f028a8686658cd3836c8b2ba5af84c55
ms.sourcegitcommit: 1c3a0f5c27ee741d26f693ad657754c4ab29ac73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71189888"
---
# <a name="msix-packaging-tool-release-notes"></a>Notas de versão da ferramenta de empacotamento MSIX

### <a name="version-120199190"></a>1\.2019.919.0 da versão
- A assinatura do Device Guard agora está disponível. Essa opção de assinatura requer uma conta do Active Microsoft Azure Active Directory configurada para o Microsoft Store para empresas. Para obter mais informações, consulte [este artigo](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing).
- O **Editor de pacote** agora dá suporte à capacidade de selecionar vários itens nos quais executar uma ação.

### <a name="version-120198080"></a>Versão 1.2019.808.0
- Melhorias nas configurações
    - Separação dos padrões da ferramenta de outras configurações
    - Capacidade adicionada para importar e exportar configurações
- Melhorias na assinatura
    - Agora você pode escolher uma opção de assinatura padrão para os fluxos de trabalho
- Atualização das etapas durante o empacotamento para melhorar a experiência

## <a name="version-120197010---public-release"></a>Versão 1.2019.701.0 – versão pública

- Suporte para instaladores de desktop que exigem reinicialização- [saiba mais](../support-restart.md)
    - Opção de logon automático para reinicialização 
- Novas opções nas configurações do aplicativo
    - Especifique um certificado padrão para assinar pacotes 
    - Especificar códigos de saída para instaladores que exigem reinicialização
    - Código de saída para a lista de reinicializações em configurações inclui valores padrão comuns
- Capacidade de adicionar recursos novos, desconhecidos e personalizados por meio do editor de pacote
- Define automaticamente MinVersion para 1709 quando os requisitos de controle de versão de armazenamento são desativados em configurações
- Novas pastas podem ser adicionadas em ativos no editor de pacotes
- Restaurar configurações padrão e itens de exclusão agora também limpa os códigos de senha e de saída do certificado de autenticação
- Corrigido um problema em que as primeiras tarefas de inicialização não estavam sendo excluídas corretamente
- Irá ignorar atalhos para arquivos excluídos durante a criação do pacote
- O padrão é assinar um pacote se um certificado de autenticação padrão for especificado nas configurações
- Honrar códigos de saída do instalador do PowerShell
- Informa ao usuário quando ele precisa de uma reinicialização para o driver
