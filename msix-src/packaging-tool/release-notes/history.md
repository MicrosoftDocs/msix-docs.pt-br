---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 2fb911d4515816a02ad74064e4f7db78efbcb5eb
ms.sourcegitcommit: f6cee51b46fc36a57b5cf9c8cb3fd24a40ae858a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391665"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas sobre a versão da Ferramenta de Empacotamento MSIX

## <a name="version-120203190"></a>1\.2020.319.0 da versão
- Qualidade aprimorada de ícones extraídos
- Extração de ícones aprimorados de atalhos
- correções de bugs gerais

## <a name="version-120202250"></a>1\.2020.225.0 da versão
- Capacidade de importar arquivos de registro (. reg) no editor de pacote
- Capacidade de adicionar automaticamente o [suporte do MSIX Core](../../msix-core/msixcore.md) a um MSIX
- Adicionar bugs corrigidos da extensão InstalledLocationVirtualization:
- Validar o formato do manifesto após a edição 
- Tornar a mensagem quando a tarefa de inicialização inicial falhar mais claro 
- Proibir caminhos relativos para desempacotamento 
- Filtros de arquivo atualizados para mostrar quais formatos são válidos (por exemplo, para instaladores que ele costumava dizer *.*  e agora `*.msi, *.exe, ...`) 
- Corrigido quando o Unpack converteria espaços em caminhos em "%20"

## <a name="version-120202030"></a>1\.2020.203.0 da versão
- A configuração de integridade do pacote está ativada por padrão
- A página Criar pacote agora mostra o local de salvamento padrão
- Capacidade de exportar seus arquivos de registro (. reg)
- Aprimoramentos de correção de bugs gerais

## <a name="version-1201912200---public-release"></a>Versão 1.2019.1220.0 – versão pública
- O suporte para a conversão de um instalador existente com serviços agora está disponível
  - Veja todos os serviços detectados e modifique-os na nova [página do Relatório de serviços](../convert-an-installer-with-services.md)
- Nova configuração para especificar o ambiente de conversão padrão
- Nova configuração para especificar o local de procura do instalador padrão
- Nova configuração para adicionar integridade de pacote a aplicativos
- Adicionar botões executar e remover à página iniciar tarefas primeiro
- Adicionada a capacidade de arrastar e soltar arquivos para movê-los no editor de pacotes
- Correção adicionada para um problema com a tradução de caminhos de servidor COM que o estavam causando inválidos
