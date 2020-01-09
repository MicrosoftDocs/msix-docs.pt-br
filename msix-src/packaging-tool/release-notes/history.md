---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 9618cbebe07696f9806fcdd07acdd8c5d834724e
ms.sourcegitcommit: 44b9510ea76623d668d87ddca575a7921c60a19a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75322611"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas de versão para a ferramenta de empacotamento MSIX

## <a name="version-1201912180"></a>1\.2019.1218.0 da versão

- Adicionada a capacidade de arrastar e soltar arquivos para movê-los no editor de pacotes
- Nova configuração para adicionar integridade de pacote a aplicativos
- Mostrar local de salvamento padrão na página Criar pacote

## <a name="version-1201910280"></a>1\.2019.1028.0 da versão

- O suporte para a conversão de um instalador existente com serviços agora está disponível
  - Ver todos os serviços detectados e modificá-los na [página novo relatório de serviços](../convert-an-installer-with-services.md)
- Nova configuração para especificar o ambiente de conversão padrão
- Nova configuração para especificar o local de procura do instalador padrão
- Adicionar botões executar e remover à página iniciar tarefas primeiro

## <a name="version-1201910180---public-release"></a>Versão 1.2019.1018.0 – versão pública

- Cadeias de caracteres atualizadas para liberação pública
- Agora a autenticação do Device Guard está disponível. Essa opção de autenticação requer uma conta do Microsoft Azure Active Directory configurada para a Microsoft Store para Empresas. Para obter mais informações, consulte [este artigo](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing).
- O **Editor de pacote** agora dá suporte à capacidade de selecionar vários itens nos quais uma ação será executada.
- Agora há suporte para o clique com o botão direito do mouse para editar um pacote MSIX.
- Melhorias da experiência do usuário para o fluxo de trabalho de empacotamento.
- Aprimoramentos de configurações:
    - Os padrões da ferramenta foram separados de outras configurações.
    - Adicionada a capacidade de importar e exportar configurações.
- Aprimoramentos de assinatura:
    - Agora você pode escolher uma opção de assinatura padrão para fluxos de trabalho.
- As etapas foram atualizadas durante o empacotamento para melhorar a experiência.

