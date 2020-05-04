---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: f156b0f6dc48a7b36c19844cd35a265b9de38f74
ms.sourcegitcommit: e650c86433c731d62557b31248c7e36fd90b381d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82726406"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas de versão para a ferramenta de empacotamento MSIX

## <a name="version-120204230"></a>1.2020.423.0 da versão
- Capacidade de adicionar itens do editor de pacote à lista de exclusões
- Usar Ctrl para opções de seleção múltipla no editor de pacote
- Adicionou um prompt ao substituir arquivos ao mover ou adicionar

## <a name="version-120204020---public-release"></a>Versão 1.2020.402.0 – versão pública
- A configuração de integridade do pacote está ativada por padrão
- Capacidade de adicionar automaticamente o [suporte do MSIX Core](../../msix-core/msixcore.md) a um MSIX
- Capacidade de importar ou exportar arquivos de registro (. reg) no editor de pacote
- A página Criar pacote agora mostra o local de salvamento padrão
- Adicionar extensão InstalledLocationVirtualization
- Qualidade aprimorada de ícones extraídos
- Extração de ícones aprimorados de erros corrigidos de atalhos:
- Validar o formato do manifesto após a edição 
- Tornar a mensagem quando a tarefa de inicialização inicial falhar mais claro 
- Proibir caminhos relativos para desempacotamento 
- Filtros de arquivo atualizados para mostrar quais formatos são válidos (por exemplo, para instaladores que ele costumava dizer *.*  e agora `*.msi, *.exe, ...`) 
- Corrigido quando o Unpack converteria espaços em caminhos em "%20"
- correções de bugs gerais

## <a name="version-1201912200---public-release"></a>Versão 1.2019.1220.0 – versão pública
- O suporte para a conversão de um instalador existente com serviços agora está disponível
  - Veja todos os serviços detectados e modifique-os na nova [página do Relatório de serviços](../convert-an-installer-with-services.md)
- Nova configuração para especificar o ambiente de conversão padrão
- Nova configuração para especificar o local de procura do instalador padrão
- Nova configuração para adicionar integridade de pacote a aplicativos
- Adicionar botões executar e remover à página iniciar tarefas primeiro
- Adicionada a capacidade de arrastar e soltar arquivos para movê-los no editor de pacotes
- Correção adicionada para um problema com a tradução de caminhos de servidor COM que o estavam causando inválidos
